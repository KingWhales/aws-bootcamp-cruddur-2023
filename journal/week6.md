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
