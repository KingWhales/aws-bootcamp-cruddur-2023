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
