# Week 4 — Postgres and RDS

I learned about RDS- PSQL database this week. I first connected psql via CLI. Created tables and inserted data into the table by running some of the bash scripts. Created the same connection in both the environments Dev and Prod.

So firstly, I installed the Postgres container which is in my docker-compose.yml file.

 db:
    image: postgres:13-alpine
    restart: always
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=<enteryourpassword>
    ports:
      - '5432:5432'
    volumes: 
      - db:/var/lib/postgresql/data

volumes:
  db:
    driver: local
Then I connect psql in my terminal by running psql -U postgres -localhost and it ask for password then I am connected to Postgres in terminal. As I mentioned above we have certain bash scripts to create tables, drop tables, insert data into tables. Before this I had set env vars in Gitpod for the Connection Url and Prod Connection Url.

./bin/db-connect to connect to the psql
#! /usr/bin/bash
if [ "$1" = "prod" ]; then
  echo "Running in production mode"
  URL=$PROD_CONNECTION_URL
else
  URL=$CONNECTION_URL
fi

psql $URL
./bin/db-create to create a new table 'cruddur'
#!  /usr/bin/bash

CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="db-create"
printf "${CYAN}== ${LABEL}${NO_COLOR}\n"

NO_DB_CONNECTION_URL=$(sed 's/\/cruddur//g' <<< "$CONNECTION_URL")
psql $NO_DB_CONNECTION_URL -c "create database cruddur;"
./bin/db-drop to drop if the table is existing
#!  /usr/bin/bash

CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="db-drop"
printf "${CYAN}== ${LABEL}${NO_COLOR}\n"

NO_DB_CONNECTION_URL=$(sed 's/\/cruddur//g' <<< "$CONNECTION_URL")
psql $NO_DB_CONNECTION_URL -c "drop database cruddur;"
./bin/db-schem-load to load the schema , which means to give the contents and set its' constraints.
#! /usr/bin/bash

CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="db-schema-load"
printf "${CYAN}== ${LABEL}${NO_COLOR}\n"

schema_path="$(realpath .)/db/schema.sql"
echo $schema_path

if [ "$1" = "prod" ]; then
  echo "Running in production mode"
  URL=$PROD_CONNECTION_URL
else
  URL=$CONNECTION_URL
fi

psql $URL cruddur < $schema_path
./bin/db-seed to insert the data into schema loaded
#! /usr/bin/bash

CYAN='\033[1;36m'
NO_COLOR='\033[0m'
LABEL="db-seed"
printf "${CYAN}== ${LABEL}${NO_COLOR}\n"

seed_path="$(realpath .)/db/seed.sql"
echo $seed_path

if [ "$1" = "prod" ]; then
  echo "Running in production mode"
  URL=$PROD_CONNECTION_URL
else
  URL=$CONNECTION_URL
fi

psql $URL cruddur < $seed_path

And to connect to PROD environment, you can suffix the command with PROD. ./bin/db-connect prod
 

![psql](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/23ad2744-603c-48eb-aaba-f4e1f7634b1d)
![schema postgres](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/775b5362-51ce-49fa-aa6b-c8b6d3fcca22)
![schema postgres](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/72ebc82a-4498-42f3-bcad-947f8c7635c9)
![bash script](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/951b4e0c-0efc-4867-89bd-6921971518f2)
![bash script db-create](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/837f0c91-59e9-492a-9d7f-897f8f0810c1)
![bash scripting](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/a3731db6-b842-43f8-8fcd-0f193313f102)
![crudding](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/38b2f7af-89e1-41b2-a2a9-39ff7bdf94c7)
![error](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/563b74e8-5763-4297-b619-b5d54fdddf76)
![seed](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/fc93ae05-e8cf-4f4e-9275-c523050b12c5)
](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/bed5bbcb-0dc7-4e94-8136-335caa246f70)
![seed script](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/96a4e07b-eb6d-498d-9d85-664d979fedc6)
![seed](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/69a88f09-5d36-48c6-ae8c-fb4a7b3dff07)
![scr5](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/b37e0e75-965a-44ec-af25-a4e623036235)
![scr6](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/a12890d8-aa26-459c-a804-dcb45ae656fe)
![rec1](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/503d2549-ef46-481d-93d7-5d5a06d62894)
![post](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/eef766a2-7073-4459-a90e-52e28c030b79)
![run1](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/96d753c6-7e90-43da-8fa6-a971bcb28539)
![run2](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/d929bd83-ab8e-4396-911c-edaeee68b208)


RDS Instance
I also created a Database instance in Amazon RDS Service. But as it costs us, so I had stoppped that temprorarily and was running only when required. I took the end point of that instance for the connection URL; security group ID and security group rules ID and added those in my rds-update-sg-rules shell script. Also had set the Inbound rules as Postgres : port 5432 to Custom : (My Gitpod IP).

There's a Provisioning done for RDS (You need to wait for arounf 10 mins to get it activated)

aws rds create-db-instance \
  --db-instance-identifier cruddur-db-instance \
  --db-instance-class db.t3.micro \
  --engine postgres \
  --engine-version  14.6 \
  --master-username root \
  --master-user-password huEE33z2Qvl383 \
  --allocated-storage 20 \
  --availability-zone ca-central-1a \
  --backup-retention-period 0 \
  --port 5432 \
  --no-multi-az \
  --db-name cruddur \
  --storage-type gp2 \
  --publicly-accessible \
  --storage-encrypted \
  --enable-performance-insights \
  --performance-insights-retention-period 7 \
  --no-deletion-protection
These all tasks helped us to get the IP from which we were creating database/ inserting data. And we used psycopg3 driver

![postgres console](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/fea6a0c0-8cb5-41ff-8247-5eea8aaa53fc)
![postgres cli](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/f5b2d273-b59b-4830-a394-da89784d80c3)
![gitpod IP ](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/f1e68d7e-912b-41dd-8459-cc2ef1859714)
![GITPOD](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/8723130a-bb44-43b0-b1ce-4ca7f34c26cd)
![GITPOD](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/8c252c5c-c67e-4aba-9b43-3a9ecada5a10)
![scripting](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/bee550a1-75c3-4d19-9cf1-418235d9a760)


AWS Lambda
Post Confirmation Lambda : Here I added some code to get logs recorded in as I sign in to the cruddur app. Created a Lambda Function by using psycopg3 lib. https://pypi.org/project/psycopg2-binary/#files

Lambda function

import json
import psycopg2

def lambda_handler(event, context):
    user = event['request']['userAttributes']
    try:
        conn = psycopg2.connect(
            host=(os.getenv('PG_HOSTNAME')),
            database=(os.getenv('PG_DATABASE')),
            user=(os.getenv('PG_USERNAME')),
            password=(os.getenv('PG_SECRET'))
        )
        cur = conn.cursor()
        cur.execute("INSERT INTO users (display_name, handle, cognito_user_id) VALUES(%s, %s, %s)", (user['name'], user['email'], user['sub']))
        conn.commit() 

    except (Exception, psycopg2.DatabaseError) as error:
        print(error)
        
    finally:
        if conn is not None:
            cur.close()
            conn.close()
            print('Database connection closed.')

    return event
    ```
    
![lambda post confirmation](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/d43848e4-9628-4b86-bce3-2ce9c946bed6)
![lambda post](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/30919366-8475-4015-9978-bac6f42936f1)
![layer](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/ae90fd70-85da-414d-9181-d18c0cce7693)
![lambda trigger](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/5404c54f-d89b-480a-98c8-f435007eb923)
![permission](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/32f7a479-215e-41e2-972f-5af001a1f9ed)
![cloudwatch](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/3e7ae162-e6d7-4bd5-bc1c-06d6aeddd10f)
![permission](https://github.com/KingWhales/aws-bootcamp-cruddur-2023/assets/111932225/b50a8f33-cdb1-4a93-81e4-9112a694f175)

