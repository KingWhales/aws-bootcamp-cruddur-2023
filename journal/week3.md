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
