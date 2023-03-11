# Week 3 — Decentralized Authentication

# Cognito User Pool setup
I created Cognito user pool manually using AWS console instead of CLI 
![User pool created](https://user-images.githubusercontent.com/111932225/223702220-298d11d9-b3ad-4711-8b11-d69420887204.png)
Next step I installed AWS amplify library by running "npm i aws-amplify --save" in the frontend-react.js directory and have it saved in package.json file
![AWS-amplify](https://user-images.githubusercontent.com/111932225/223723089-3e294438-ce58-422c-a303-1dd793d3e87c.png)

I hooked up our cognito pool to  our code in App.js file by running this command: import { Amplify } from 'aws-amplify';
![cognito pool](https://user-images.githubusercontent.com/111932225/223725004-adab77a9-b174-4b07-9d30-a77f79e5aed2.png)

Went ahead and configure amplify in both App.js and docker-compose.yml file
![amplify configuration](https://user-images.githubusercontent.com/111932225/224047327-ca3eae88-bd0e-4e02-9c1d-1cd180ddab0c.png)
![amplify config](https://user-images.githubusercontent.com/111932225/224047336-4c181dc8-6cd5-4726-8424-07d56fee843e.png)

I updated HomeFeedPage configuration
I updated the ProfileInfo configuration
Before I move on to configuring my signing page configuration, I tried to sign in with wrong details to see if I would get "incorrect password" in which I did
![incorrect username and password](https://user-images.githubusercontent.com/111932225/224064542-de7595e7-6881-415d-b0e3-8b1298debdbd.png)
I created a user in userpool console and run the command below to have my signing authenticated in Cruddur app
![command](https://user-images.githubusercontent.com/111932225/224318264-9a7987d9-21f6-4133-84d3-1a8035e77636.png)

Signed in to Cruddur with correct details
![cruddur signin](https://user-images.githubusercontent.com/111932225/224477841-188c6b6d-8cba-4fa6-91a6-fc6316076933.png)
Configure the sign up file and clicked on sign up to see if it works and it did work
![cruddur confirmation](https://user-images.githubusercontent.com/111932225/224494225-ab8fdd52-3fee-4842-b7a8-c23be22750f4.png)
![signed into cruddur](https://user-images.githubusercontent.com/111932225/224494245-0793a44b-88e1-4f14-8d21-6785c1844133.png)
