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

https://lucid.app/lucidchart/0de49364-9cfe-48b8-8b4f-985c6ba6f9ca/edit?viewport_loc=-912%2C-622%2C2101%2C812%2C0_0&invitationId=inv_d7371682-292f-436d-a193-b01e6df2c4f2

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


# Conceptual Architecture Diagram (Napkins)

![IMG_3178](https://user-images.githubusercontent.com/111932225/219863669-da0bddda-7811-4214-87c4-6b6a5b1585bb.jpg)


# Reviewing all the questions of each pillars in the Well-Architected Tool

I navigated to Well Architecture Framework tools and defined a workload and name it cruddur and I put Andrew Bayco as Review ower and I chose N. Virgina where I want my workload to run. I input my account ID, chose other as my industry type then I typed "Social media" to be in the industry section. I clicked next then selected AWS Well-Architected framework as my Lens, then I clicked on "Define workload" and the clicked "Start reviewing"

![Define Well Architected Framework](https://user-images.githubusercontent.com/111932225/219873342-dda7db55-c6d8-4819-bab5-4eb08e06ece2.png)
![AWS Well-Architecture Frameworl](https://user-images.githubusercontent.com/111932225/219873362-1b14be1d-38aa-440f-970d-8e76f0147e8e.png)

# USES OF C4 MODELS

C4 (Context, Containers, Components, Code) is a visual modeling approach for software architecture that provides a way to represent and communicate the design of a software system. Here are some of the uses of C4 models:

Communication: C4 models can be used as a communication tool to help different stakeholders understand the structure of a software system. By using C4 models, teams can discuss and agree on the high-level design of the system and ensure that everyone is on the same page.

Analysis: C4 models can be used to analyze the structure of a software system and identify potential issues and areas for improvement. For example, the models can be used to identify components that are tightly coupled and may need to be decoupled to improve maintainability.

Documentation: C4 models can be used to document the architecture of a software system. By using the models, teams can create a visual representation of the system that can be used as a reference for future development work.

Evolution: C4 models can be used to support the evolution of a software system over time. By updating the models to reflect changes in the system, teams can ensure that the architecture remains consistent and understandable.

Planning: C4 models can be used to support planning activities, such as estimating the effort required to develop or modify a software system. By understanding the structure of the system, teams can make more accurate estimates and plan more effectively.

# LAUCHING AWS CLOUD SHELL AND LOOKING AT AWS CLI

AWS clouyd shell is easy to launch as the icon is located right top corner of the console, it is the icon highlighted in red below
![cloud shell](https://user-images.githubusercontent.com/111932225/219874812-98a403ae-9e2e-44ef-b5a3-33c5a2ff9015.png)

Once you click the icon you get a welcome greeting display
![Welcome to cloudshell](https://user-images.githubusercontent.com/111932225/219875043-4bcb8d67-7b40-40bd-bf16-a4a1b4c37db0.png)

I ran few commands just to make sure it is working

![cloudshell](https://user-images.githubusercontent.com/111932225/219875282-71835aff-80bd-432e-8992-cb939dc1c821.png)
