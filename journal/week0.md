# Week 0 — Billing and Architecture
# Discussion in our week 0 class
Tutor talked about monolithic and microservice architecture, she went further to explain benefit of microservice application to us, by making us understand it has agility, flexible scaling, easy deployment benefit and technical freedom benefits. She sketched us a triangle diagram which shows the SCOPE, COST and TIME that will affect the project stakeholders and the members.
She explained what Good Architecture is and listed the attributes that make up a good architecture, she listed how to meet the project requirements,  addressed the Risk, Assumption and Constraints, info gathered from RACs to create your designs (conceptual, logical and physical design), to develop a common dictionary(asking dumb questions, play be-the-packet, documentation) and use of AWS Well-Architected Framework  to review your workloads against current AWS best practice (Operational excellence, Performance efficiency, Reliability,  Cost Optimization, Security and Sustainability).

# Setting up Billing alarm
I had to login into my root account to grant my User account access to the  billing console
![user permission](https://user-images.githubusercontent.com/111932225/219644113-a1edccb2-a005-455f-b182-45d392db7241.png)
I logged in from another browser which is Firefox to save  me the stress of logging out of my User account, then  I granted my user account permission to access billing console
![Permission granted](https://user-images.githubusercontent.com/111932225/219645646-f096f588-d721-4c3f-90f7-df0b12f9d33f.png)
I went ahead to enable billing alert first before I can set up billing alarm, I clicked on billing preference and put in the email address I want to receive alert with
![billing preference](https://user-images.githubusercontent.com/111932225/219647064-a108da5a-52e2-4700-b983-4e65421d93ab.png)
Then I proceeded to cloudwatch console to create billing alarm
![creating billing alarm](https://user-images.githubusercontent.com/111932225/219654075-f4ad1c16-d03c-469c-a8c8-d34e06de65f3.png)
I clicked on next to create SNS topic for notification
![Setting up SNS topic](https://user-images.githubusercontent.com/111932225/219654351-66237895-83d7-4ffe-8b88-642dfe0b1980.png)
Then I logged into my email to confirm the subscription, then I clicked next to give the alarm a name and description
![name annd description](https://user-images.githubusercontent.com/111932225/219655891-1f5b7951-c598-4356-b1cc-0c4d4729eb69.png)
I clicked next to see "Preview and create" page before I clicked on "create alarm"
![alarm created](https://user-images.githubusercontent.com/111932225/219656532-3a3fe95e-13b3-427a-b3d2-0c86c0f83eca.png) deb81bace18

# USING LUCID CHART ARCHITECTURE DIAGRAM

![Lucidchart](https://user-images.githubusercontent.com/111932225/219817872-f9382241-1fe6-4480-9d66-80d6f58fbfbf.png)


# USE EVENTBRIDGE TO HOOKUP HEALTH DASHBOARD TO SNS AND SEND NOTIFICATION WHEN THERE'S A SERVICE HEALTH ISSUE

I searched for Health Dashboard and click on configure Eventbridge from Health Dashboard

![health dashboard](https://user-images.githubusercontent.com/111932225/219857804-94ce2b61-5eed-438a-b4ec-607100ff1d69.png)

Chose Health as event source 

![health as event source](https://user-images.githubusercontent.com/111932225/219857955-ad5b88ed-cff9-4f82-9460-6207b97a2c7c.png)
![health as event source](https://user-images.githubusercontent.com/111932225/219857959-c2594b31-9704-445a-8226-29f2bccf3ad1.png)

Then I selected SNS to invoke when there's an event match, skipped tag page and the click create rule

![SNS as event source](https://user-images.githubusercontent.com/111932225/219858051-a2a4daec-dbf4-4991-8cc5-899d50f8b109.png)
![SNS to invoke](https://user-images.githubusercontent.com/111932225/219858069-0f5e5ef8-7095-4bf3-87d3-6865a4f29ace.png)
