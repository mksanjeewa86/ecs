## The Node.js app to deploy
Enter the following in your terminal:
create a new directory
```
mkdir ecs-nodejs-app
```
change to new directory
```
cd ecs-nodejs-app
```
Initialize npm
```
npm init -y
```
install express
```
npm install express
```
create an server.js file
```
touch server.js
```
Open server.js and paste the code below into it:
```
server.js

const express = require('express')
const app = express()

app.get('/', (req, res) => {
res.send('Welcome from a Node.js app!')
})

app.listen(3000, () => {
console.log('Server is up on 3000')
})
```
Start the app with:
```
node server.js
```
Access it on http://localhost:3000. You should get Welcome from a Node.js app! displayed in your browser.
Next, we’re going to dockerize our app.
## Writing a Dockerfile
We are going to start dockerizing the app by creating a single file called a Dockerfile in the base (root) of our project directory. This file has no file extension.
```
FROM node:8-alpine
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app
COPY . .
RUN npm install
EXPOSE 3000
CMD [ "node", "server.js" ]
```
## Building a Docker image
```
docker build -t ecs-nodejs-app .
```
## Running a Docker Container
We’ve built the docker image. To see previously created images, run:
```
docker images
```
Copy the Image Id. To run the container, we write on the terminal:
```
docker run -p 80:3000 {image-id}
// fill with your image-id
```
Here we publish the app to port 80:3000. Because we are running Docker locally, go to http://localhost to view.
## Create the registry and push your app image there
Amazon Elastic Container Registry (ECR) is a fully-managed Docker container registry that makes it easy for developers to store, manage, and deploy Docker container images. Amazon ECR is integrated with Amazon Elastic Container Service (ECS), simplifying your development to production workflow.

Before we get to pushing up our app image, ensure that your AWS CLI can connect to your AWS account. Run on the terminal:
```
aws configure
```
If your AWS CLI was properly installed, aws configure will ask for the following:
```
aws configure
AWS Access Key ID [None]: <accesskey>
AWS Secret Access Key [None]: <secretkey>
Default region name [None]: us-east-2
Default output format [None]:
```
