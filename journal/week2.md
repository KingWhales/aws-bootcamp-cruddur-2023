# Week 2 — Distributed Tracing

# HONEYCOMB
I got to know about honeycomb as a distributed tracing service and I got to implement it with the help of Andrew and Jessica. Honeycomb is used for logging and created data. I created an account with Honeycomb as I didn't have an account, and created an environment and used that environment's API Key to connect my Cruddur application data with Honeycomb, I followed the guidance as demonstrated in the youtube video and I was able to create an environment and connect it with Honeycomb for data logging and tracing. Backend flask application was instrument to use Open Telemetry(OTEL) with Honeycomb as the provider, then I ran quries to explore traces within Honeycomb 

![honeycomb datasets](https://user-images.githubusercontent.com/111932225/222900213-7fb78766-38dd-4510-9b17-f15da347d1c4.png)
![honeycomb tracing](https://user-images.githubusercontent.com/111932225/222900215-f4321596-1fe9-4e39-8c73-39496389e1db.png)

# X-RAY
I followed the youtube video and instrument AWS X-RAY into backend flask application, configure anf provision X-RAY daemon within docker-compose and send data back to X-RAY API
![xray daemon configuration](https://user-images.githubusercontent.com/111932225/222902301-eedafb6c-51a5-498c-a935-4aed0726f9df.png)
![XRAY Cruddur created in console](https://user-images.githubusercontent.com/111932225/222902328-19a0f81c-db0d-4b4b-b446-3d24f8c8c12c.png)
![xray works](https://user-images.githubusercontent.com/111932225/222902341-ddedb91a-150f-4bc5-95e7-40935dabd65f.png)


# CLOUDWATCH
I installed watchtower by pasting watchtower in requirement.txt in backend flask application and change directory into backend flask to ran "pip install -r requirement.txt" to have the watchtower installed, then I imported watchtower and logging in app.py file and I configured logger to use cloudwatch by pasting the logger configuration commands in app.py file, then set environment variables by pasting the commannds in docker-compose.yml file to access cloudwatch and then compose it up then logged into my aws account and nnavigated to cloudwatch to see if it's shwoing on there and it was
![cloudwatch logs](https://user-images.githubusercontent.com/111932225/222904381-454fc54d-e1f3-49fd-975d-4e07830977ad.png)
![cloudwatch log streams](https://user-images.githubusercontent.com/111932225/222904418-d0fb5fb4-e61d-4dd2-a4a2-ecd0d7f6330a.png)
![cloudwatch log event](https://user-images.githubusercontent.com/111932225/222904422-b9def0ca-f2bf-48f8-b6a0-5da04ec3ae2a.png)


# ROLLBAR
First time I'd be hearing about Rollbar, I got to understand Rollbar is use to log, trace and monitor errors. I had to start the installation by pasting "blinker rollbar" in "requirement.txt" under backend-flask folder then change directory into backend-flask in terminal and run "pip install -r requirement.txt" to have Blinker Rollbar installed, then proceed to set my access token, had to create a new text file and export and set my environment for the Roll bar access key, copied the access from my rollbar account and paste it in the new file created then run export ROLLBAR_ACCESS_TOKEN="7df711f21e6c47dd8cb5eb4b821f9d65" gp env ROLLBAR_ACCESS_TOKEN="7df711f21e6c47dd8cb5eb4b821f9d65" in the terminal to set the variable and run env | grep Rollbar to make sure it os set
![rollbar](https://user-images.githubusercontent.com/111932225/222910232-6d7b1e50-9960-486d-94ce-31ad15e558a3.png)
I ran the Rollbar configuration endpoint to get it working
![rollbar endpoint](https://user-images.githubusercontent.com/111932225/222965656-7beacb2e-aca4-442a-baee-48c25a0995aa.png)

I navigated back to Rollbar to see if it has picked up the trace of the endpooint and I also tried to make an error to see if rollbar would pick it and show it and it did
![rollbar item](https://user-images.githubusercontent.com/111932225/222969480-70e41490-76be-4771-92bd-d7fa795bf5c5.png)
![rollbar error](https://user-images.githubusercontent.com/111932225/222969493-209e1635-3680-438c-b6dd-8c4b4f7a514d.png)
