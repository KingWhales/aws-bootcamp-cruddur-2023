# Week 1 — App Containerization

I followed through Andrew videos to create my backend and frontend for Cruddur app, I installed node package in my frontend directory to have the port 3000 running and then I composed up in my in my docker-compose.yml directory to start the app and I opened up the three ports to grant me access to the website

![ports](https://user-images.githubusercontent.com/111932225/221354928-807cca37-7846-446e-af8b-e902fb710483.png)

# Here is the end result after clicking the link on port 3000

![home page](https://user-images.githubusercontent.com/111932225/221352547-4ee64ee1-b8e2-4e12-a7c9-9ee3b87f9dce.png)
![login ](https://user-images.githubusercontent.com/111932225/221352653-2d42f648-83c4-4ce5-ab7b-b49473800f78.png)
![notification page](https://user-images.githubusercontent.com/111932225/221352658-54324354-c4d3-4ca1-8708-ab718f02bdb6.png)

# I followed through with Andrew DynamoDB and Postgres vs Docker video to have my DybamoDB and Postgres created

![database](https://user-images.githubusercontent.com/111932225/221355519-de3816fa-d9c5-4ed5-8bdb-efac4db5d57a.png)


# HOMEWORK CHALLENGE
I ran the Dockerfile CMD as an external script 
![dockerscript](https://user-images.githubusercontent.com/111932225/223159310-e4d4a9d0-5512-4c58-ab40-2a432c05c7a8.png)

I pushed and tagged an image to Dockerhub by logging in with "docker login" command
![docker login](https://user-images.githubusercontent.com/111932225/223160326-5dbf17db-a4ef-4b2e-912e-81ac0cfc6e53.png)
Then I run this command "docker tag hello-docker kingwhales/my-repo" to identify the image i want to push to docker and then run this command "docker push kingwhales/my-repo" to have it pushed 
![docker push and tag](https://user-images.githubusercontent.com/111932225/223161453-0451c691-2e0d-4c65-ae0d-38baecfa943e.png)

