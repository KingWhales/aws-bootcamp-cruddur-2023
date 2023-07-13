# Week 6 — Deploying Containers
 In container, we have diferrent option for deployment, we can deploy containers to lambda, to appRunner, to elastic beanstalk, to ECS fargate, EC2 fargate, ECS EC2 and kubernetes. We make use of fargate because it's easier to use and it's more modern architecture compare to ECS EC2.
  We need to setup a health checkto be sure the state of ECS, so we had to setup a command in backend-flask>bin>db folder and name the file "test", then we make it excutable by running the below command in terminal under backend-flask directory
 chmod u+x bin/rds/update-sg-rule
Then we execute test file to confirm if the connection is successful
![test script](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/0cb29ac3-2323-4170-ab5a-6ccad3b864dc)
Then we need to have health check for our flask app, so we went ahead to paste the code below in backend-flask>app.py file:

```
@app.route('/api/health-check')
def health_check():
  return {'success': True}, 200
```

We need to make a new script for health-check, we created a script file in backend_flask>bin>flask>health-checkand write in the below code

```
#!/usr/bin/env python3

import urllib.request

try:
  response = urllib.request.urlopen('http://localhost:4567/api/health-check')
  if response.getcode() == 200:
    print("[OK] Flask server is running")
    exit(0) # success
  else:
    print("[BAD] Flask server is not running")
    exit(1) # false
# This for some reason is not capturing the error....
#except ConnectionRefusedError as e:
# so we'll just catch on all even though this is a bad practice
except Exception as e:
  print(e)
  exit(1) # false
```

Then we make the health-check script executable

We created a cloudwatch named "cruddur/fargate-cluster" and set retention time to 1 day

```
aws logs create-log-group --log-group-name "cruddur/fargate-cluster"
aws logs put-retention-policy --log-group-name"cruddur/fargate-cluster" --retention-in-days 1
```
![cloudwatch created](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/55da6547-c95e-4e38-930e-b91b059908dd)

Then we moved to creating ECS cluster with the be below command:

```
aws ecs create-cluster \
--cluster-name cruddur \
--service-connect-defaults namespace=cruddur
```
![ecs cluster created](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/16fa169c-a5ae-4424-9bb5-a054629449ab)


Next step is to create ECR repository and we name it "cruddur-python" and set image tag as "MUTABLE"  with the below command:
```
aws ecr create-repository \
  --repository-name cruddur-python \
  --image-tag-mutability MUTABLE
```
![cruddur-python](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/55564eeb-9c76-4353-ad3f-30bfa22361b9)
![cruddur-python](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/c000a34a-8ead-4a10-941e-b62758588c1a)

then proceed to login to ECR with the push command shown on your on your AWS console UI after you check-box the ECR repository, you copy that and paste in your terminal and you get "login succeeded" response which means you can now push container. We go ahead and set  URL in our terminal by exporting it with this command:

`export ECR_PYTHON_URL="$AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/cruddur-python"`

We go ahead and pull docker image in backend environment with the following command:
docker pull python:3.10-slim-buster

then we tag  our image with the following command:

`docker tag python:3.10-slim-buster $ECR_PYTHON_URL:3.10-slim-buster`
 
then push the image with the following command:

`docker push $ECR_PYTHON_URL:3.10-slim-buster`

We need to update our flask app to our repository URL endpoint in our Docker file

Next step is to create another ECR repository and we name it "backend-flask" and set image tag as "MUTABLE"  with the below command:
```
aws ecr create-repository \
  --repository-name backend-flask \
  --image-tag-mutability MUTABLE
```

We go ahead and set  URL in our terminal by exporting it with this command:

`export ECR_PYTHON_URL="$AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/backend-flask"`

We first need to go to create a paramenter before we create execution role by passing sensitive data to Task Definition with these commands:
```
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/AWS_ACCESS_KEY_ID" --value $AWS_ACCESS_KEY_ID
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/AWS_SECRET_ACCESS_KEY" --value $AWS_SECRET_ACCESS_KEY
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/CONNECTION_URL" --value $PROD_CONNECTION_URL
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/ROLLBAR_ACCESS_TOKEN" --value $ROLLBAR_ACCESS_TOKEN
aws ssm put-parameter --type "SecureString" --name "/cruddur/backend-flask/OTEL_EXPORTER_OTLP_HEADERS" --value "x-honeycomb-team=$HONEYCOMB_API_KEY"
```

![parameter](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/9589e52f-3bb0-460d-a763-df90d5ab2a74)
![parameter console](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/5361e6df-cb65-4c49-8d8c-dfbd630df9d6)

We create Task and Execution role with the below command:
```
aws iam create-role \    
--role-name   \   
--assume-role-policy-document file://aws/json/policies/service-assume-role-execution-policy.json
```
![role](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/4f7c1e90-bbfa-4de2-bdf7-58751f08c517)
![role](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/252ff036-8852-40fb-b904-678245fa06bb)

We then create a Task role with the bellow code:

```
aws iam create-role \
    --role-name CruddurTaskRole \
    --assume-role-policy-document "{
  \"Version\":\"2012-10-17\",
  \"Statement\":[{
    \"Action\":[\"sts:AssumeRole\"],
    \"Effect\":\"Allow\",
    \"Principal\":{
      \"Service\":[\"ecs-tasks.amazonaws.com\"]
    }
  }]
}"
```

![task role](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/2be46e58-17e0-475d-bd73-8dda28f29bac)
![task role](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/bf320f82-b4e7-4b9d-b8bf-b26e1413e230)

Then I created a policy to use System manager with the following command:
```
aws iam put-role-policy \
  --policy-name SSMAccessPolicy \
  --role-name CruddurTaskRole \
  --policy-document "{
  \"Version\":\"2012-10-17\",
  \"Statement\":[{
    \"Action\":[
      \"ssmmessages:CreateControlChannel\",
      \"ssmmessages:CreateDataChannel\",
      \"ssmmessages:OpenControlChannel\",
      \"ssmmessages:OpenDataChannel\"
    ],
    \"Effect\":\"Allow\",
    \"Resource\":\"*\"
  }]
}
"
```

Then I give the CruddurTaskRole full access to Cloudwatch with the following code:
```
aws iam attach-role-policy --policy-arn arn:aws:iam::aws:policy/CloudWatchFullAccess --role-name CruddurTaskRole
```

And also grant AWSXRayDaemonWriteAccess to CruddurTaskRole with the  following code:
```
aws iam attach-role-policy --policy-arn arn:aws:iam::aws:policy/AWSXRayDaemonWriteAccess --role-name CruddurTaskRole
```
![permission policy](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/1af695b1-714f-47d7-966e-42c99046801f)

Created a folder named task-definitions under aws folder and created backend-flask and frontend-react-js file under it and do the necessary configuration on both files

To update the new values I recently added in ECR then I need to run the following code:
```
aws ecs register-task-definition --cli-input-json file://aws/task-definitions/backend-flask.json
```
![value updated](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/561de2ec-e762-4bf0-b11b-82b25c86da82)

To create a service for cruddur cluster, I need to have a vpc, so I used default vpc by running the code below:
```
export DEFAULT_VPC_ID=$(aws ec2 describe-vpcs \
--filters "Name=isDefault, Values=true" \
--query "Vpcs[0].VpcId" \
--output text)
echo $DEFAULT_VPC_ID
```

Then proceeded to set up security group named "crud-srv-sg" for the cruddur cluster:
```
export CRUD_SERVICE_SG=$(aws ec2 create-security-group \
  --group-name "crud-srv-sg" \
  --description "Security group for Cruddur services on ECS" \
  --vpc-id $DEFAULT_VPC_ID \
  --query "GroupId" --output text)
echo $CRUD_SERVICE_SG
```

And then authorize port 80 for the security group with the following code:
```
aws ec2 authorize-security-group-ingress \
  --group-id $CRUD_SERVICE_SG \
  --protocol tcp \
  --port 80 \
  --cidr 0.0.0.0/0
```

![vpc sg port](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/6926c99c-5feb-45de-b226-25a5fcf165b3)

After the implementation to use default vpc and creation of security group and authorisation of port 80, I proceeded to create a service named "backend-flask" with this command:
```
aws ecs create-service --cli-input-json file://aws/json/service-backend-flask.json
```

![backend-flask](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/571d58da-2c0f-413f-aa4f-dbc91290e4dc)

Granted CruddurServiceExecutionRole a cloudwatch full access

Then input the code below to connect to container:
```
aws ecs execute-command  \
--region $AWS_DEFAULT_REGION \
--cluster cruddur \
--task c4e823e65cba4526a43eff0a2d72fd7d \
--container backend-flask \
--command "/bin/bash" \
--interactive
```
![container connected](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/5b1284c2-5df5-4f0e-8ad3-7dc51cde967a)

I run ./bin/flask/health-check to confirm if the Flask srever is running
![server is running](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/ae750a7b-3fff-4082-9767-60307a631f02)

Created a bash script to connect to container to make it easy to run the connect, the script is named "connect-to-service" and its under the backend-flask/bin/ecs directory. 
Next is creating a load balancer to route our traffic to the healthy server, so I copied the loadbalancer command line and paste in aws/json/service-backend-flask.json file:
```
"loadBalancers": [
        {
            "targetGroupArn": "",
            "loadBalancerName": "",
            "containerName": "",
            "containerPort": 0
        }
    ]
```
I edited the inbound rule of the security group port range use with load balancer to port 4567 and 3000 to allow traffic passage to backend-flask and frontend-react-js respectfully. Checked my target group status and make sure it stays healthy.
Decided to turn on Access Logs in load balancer for detailed logs for easy debuuging, S3 bucket has to be created in the process so one was created and it's named  it "cruddur-alb-access-log"

![cruddur-alb-access-log](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/4f18a909-5b60-47a5-9c31-3a3e6efaa8e1)

Changed into frontend-react-js directory and run "npm run build". It's necessary to do that to create a production-ready version of your app that can be deployed to server. Then proceed to build an image also in the frontend-react-js directory with the below command: 
```
docker build \
--build-arg REACT_APP_BACKEND_URL="https://cruddur-alb-579528490.us-east-1.elb.amazonaws.com:4567" \
--build-arg REACT_APP_AWS_PROJECT_REGION="$AWS_DEFAULT_REGION" \
--build-arg REACT_APP_AWS_COGNITO_REGION="$AWS_DEFAULT_REGION" \
--build-arg REACT_APP_AWS_USER_POOLS_ID="us-east-1_5O8LHGtGK" \
--build-arg REACT_APP_CLIENT_ID="7sa8cle3uctvjmv0r6u61eshm5" \
-t frontend-react-js \
-f Dockerfile.prod \
.
```
I created ecr repository for frontend-react-js with the below command:
```
aws ecr create-repository \
  --repository-name frontend-react-js \
  --image-tag-mutability MUTABLE
```

I pushed image to the ECR frontend-react-js with the below command:
```
docker push $ECR_FRONTEND_REACT_URL:latest
```
Then run the below command to creat task definitions:
```
aws ecr register-task-definition --cli-input-json file://aws/task-definitions/frontend-react-js.json
```
![frontend task](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/b60d8617-b973-43e8-bf94-6986e311e6a0)
![frontend task](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/7d525c2c-b3d3-4148-a1c4-672ee36f2b3a)

Then proceed to create ecs service with the below command:
```
aws ecs create-service --cli-input-json file://aws/json/service-frontend-react-js.json
```
![ecs services for frontend](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/c9805348-25ba-4b10-b47c-04decc214be9)
![ecs services for frontend](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/35f2efe2-3685-4b15-aaea-e31d7308f5e8)

I proceeded to start custom domain, I already have one created with Route53 which  is whalesproject.co.uk, I updated nameservers for my domain and went ahead to request Certificate Manager for the encryption of my domain, I proved the ownership by dns validation by creating a CNAME record in ACM UI

![ACM](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/3de9988b-331e-46be-85ea-aa540a2a6c8e)

Went to load balancer listeners and rules to add listeners thats going to forward from port 80 to port 443 then add another listener that will forward from port 443 to cruddur-frontend-react-js target group then added my Certificate in the "Secure listener settings"

![listeners and rules](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/59050742-4a15-4bdb-9409-35aa573ce355)

Navigated to HTTPS:443 to manage the rules and have the condition set host header to "api.whalesproject.co.uk and forward it to "cruddur-backend-flask-tg" target group

![host header](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/b9675759-5c1c-45e3-a78b-e46b4f43207f)

I navigated to Route53 and then to hosted zone to create a record that points to the load balancer, I did it for  both naked whalesproject.co.uk and api.whalesprroject.co.uk domain

![LB](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/5e78a0f8-fe83-4251-90f7-f3f9fdf388c7)
![LB2](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/7499732d-7368-44fe-851e-de0be5c91ee9)
![LB3](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/3ad067da-d97c-4b38-a47d-a38474007dea)
![ping domain](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/1b2e5798-f37d-4f3a-a9a9-c2845a20a6a8)
![whalesproject](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/bbd889a3-88a6-4726-b188-e73e2295568a)

I had to set up the URL again with the below command to be able to update my frontend:
```
export ECR_FRONTEND_REACT_URL="$AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/frontend-react-js"
echo $ECR_FRONTEND_REACT_URL
```

Since I linked my domain(whalesproject.co.uk) to the frontend, I had to build the frontend image again with the updated commands in the frontend directory:
```
docker build \
--build-arg REACT_APP_BACKEND_URL="https://api.whalesproject.co.uk" \
--build-arg REACT_APP_AWS_PROJECT_REGION="$AWS_DEFAULT_REGION" \
--build-arg REACT_APP_AWS_COGNITO_REGION="$AWS_DEFAULT_REGION" \
--build-arg REACT_APP_AWS_USER_POOLS_ID="${AWS_COGNITO_USER_POOL_ID}" \
--build-arg REACT_APP_CLIENT_ID="7sa8cle3uctvjmv0r6u61eshm5" \
-t frontend-react-js \
-f Dockerfile.prod \
.
```

I tagged the image with the below command:
```
docker tag frontend-react-js:latest $ECR_FRONTEND_REACT_URL:latest
```

Then I ran the below command to push the Image:
```
docker push $ECR_FRONTEND_REACT_URL:latest
```
![pushed](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/ab77f5a3-e5e1-4ab7-823f-d91f7f862dd3)

I navigated to ECS UI to update both the backend and the frontend services by ticking the "force new deployment" box so it pick up the new changes

I edited my security group source to my IP to limit the level of accessto my computer alone since flask debug mode is on and I deleted the inbound rule on both port 3000 and 4567 since the ports are no longer use 

I created a new file named 'Dockerfile.prod' under backend-flask directory and indicate the debugger to be off since it's production. Just to confirm if the  debug mode is off in production, decided to build a docker image in production by running:
```
docker build -f Dockerfile.prod -t backend-flask-prod .
```

I created bash script for both  backend-flask and frontend-react-js to build docker image under backend-flask/bin/docker/build directory
