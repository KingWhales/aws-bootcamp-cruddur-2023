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
