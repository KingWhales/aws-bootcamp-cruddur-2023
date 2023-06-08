# Week 5 — DynamoDB and Serverless Caching
DynamoDB Data Presentation with GSI and LSI

GSI: eventually consistent global secondary index. We can choose subsets of attributes from the base table to project and it acts as another optimised table for a specific access patterns allowing to save on cost due to attribute optimisation. Items in GSIs do not need to be unique and that’s why we cannot use get_iem() operation but can only query or scan items. Another way to describe a GSI is it is a different view of the same data.
LSI: strong consistent local secondary index where data stored and ordered in a different way compared to the base table. We can slice and dice data whatever way we want to support our access patterns. LSI is created at table creation time and cannot be created or deleted afterwards.
Base Table Design
Base table shall be designed to support as many access patterns as possible while keeping amount of GSI and LSI to minimum. We shall solve 80-90% access patterns with the base table and 20-10% with GSI ( and maybe LSI).

Base table + GSI can be a comparable Dynamo DB cost for having two DynamoDB tables for our application.

Kirk said, “with RDS we model tables as database wants it whereas NoSQL database allow to model as application will interact with the data”.

DynamoDB Constraints
partition key is mandatory for querying table. We need to design base table in a way that data is evenly distributed across partition key values
if a table was created with a sort key then we can use it for querying. Sort key is optional when querying DynamoDB table

# Install Boto3
add boto3 in requirements.txt
pip install it.
run docker-compose up to run DynamoDb on local containers.

inside bin create folder 'db' and move all scripts starting with 'db-' there, and then remove this prefix.

inside bin create folder 'rds' and move script that updates default RDS security group in that folder.

setup script needs updates

inside bin create folder 'ddb' and navigate there.

create scripts inside 'ddb' folder:

schema-load
delete-table
seed
list-tables


# Implement Schema Load Script
![schema load](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/b7f1ec97-d73b-468e-a36f-04767ed99189)

# Implement Seed Script
![seed implement](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/bf55dfa5-64e7-40bc-a1a8-04b724a016c8)
![seed implementation](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/e1c0c121-12b4-4fc4-9914-0e1bc3482973)
![seed implementation](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/dd9c7a36-3a89-45c1-88f4-1cfd06b61e95)

# Implement Scan Script
![scan](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/9be148a0-5329-4f46-91ed-5026afb1b267)
![scan](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/b939fc99-47b0-419a-b6f8-af3140a47e4f)

# Implement Pattern Scripts for Get and List Conversations 
![list](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/aff392f8-6987-4e27-94ae-fdc7d68adbb5)
![get](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/c805db61-2c5b-483f-84f2-4afd9032ba26)
![get](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/33f14ce7-6462-42c7-9d7b-0e607f981e84)

# Cruddur Patterns:
# Implement (Pattern A) Listing Messages in Message Group into Application.
![pattern A](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/1cb19553-183a-435a-938f-b4f767dab2ed)

# Pattern B: Show all messages that are in a conversation and sort messages by ‘created_at’ attribute in descendent order.
![pattern B](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/21d94e8a-791d-4a2b-9910-3e63e42ea309)

# Implement (Pattern C) Creating a Message for an existing Message Group into Application
![pattern C](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/ac6ecace-eafe-44ad-86dc-7ac1c6d86a66)

# Implement (Pattern D) Creating a Message for a new Message Group into Application
![cruddur dynamodb](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/d9dc1585-f652-47bd-b173-d8dd79547321)

# Implement (Pattern E) Updating a Message Group using DynamoDB Streams
![dynamodb table](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/5d3f3a38-cdda-4a29-9811-f7be760344dd)
![turned on dynamodb stream](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/a72ac55f-f32f-403c-ab0d-f2d8f68b8017)
![created trigger for dynamodb](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/b7095810-f53e-44f4-a131-800706d8d0ea)
![dynamodb cloudwatch](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/ade1053e-61ce-4cee-8ad9-caaa122e6b8c)
![cruddur dynamodb](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/d9dc1585-f652-47bd-b173-d8dd79547321)

