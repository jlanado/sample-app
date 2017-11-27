This example stack will provision several containers as follows:
----------------------------------------------------------------
- Python web application that will let you select between two choices
- Redis queue  (key:value datastore) to collect the inputs
- .NET worker app which takes the input and store it to a database
- Postgres database where it stores the data
- Node.js web application that will render the results of selection in real-time


The workflow are as follows:
----------------------------
1. Using a web browser, user will select their choice (Democrats or Republicans) provided by the Python app

2. The Redis node will then queue/collect the selection

3. The worker app (.Net app) will then consume the selection from Redis and then store it to the Postgres DB

4. The Node.js app will pick up the data in the Postgress DB and render it on a web page in real-time


To run the "stack", a docker-compose.yml was provided and will be executed as:
------------------------------------------------------------------------------
1. From the docker host: cd ~/sample-app/ ; docker-compose up 

(This will download all "generic" images from dockerhub and build all the necessary images based on the dockerfile included in the sample-app directory, it will also start each containers running microservices on them. One you see the message "worker_1  | Connecting to redis", proceed to step 2)

2. Open a web browser: Go to http:<your_dockerhost_ip_address>:5000 (This is the Python app, make a selection)

3. Open another web browser or tab: Go to http:<your_dockerhost_ip_address>:5001 (This is the Node.js app that will show the result in real-time)

4. The application only accepts one selection per client, it does not register another selection if submitted from the same node/ip-address


To Reset:
---------
1. Kill and delete all stack instances: docker kill $(docker ps -aq) ; docker rm $(docker ps -aq)

2. Remove all stack images: docker rmi -f $(docker images | tail -n +2| awk '{print $3}')


Assumptions/Disclaimer:
-----------------------
- Before running the "stack", ensure that docker engine is installed and functional (run "docker -v" to see if docker is installed and "systemctl status docker" if it's running)

- Ensure that docker-compose binary is installed (if not, follow the instructions found in https://docs.docker.com/compose/install/#install-compose for a specific OS you are running)

- This is only a sample application and should not be used in production, it is intended to demo micro-services application from linked containers on a single docker host
  










