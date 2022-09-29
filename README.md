 ![Texte
   alternatif](https://upload.wikimedia.org/wikipedia/commons/7/79/Docker_%28container_engine%29_logo.png)

## This repository contains the projects and exercises that I was able to perform on Docker during my training at Eazytraining

## :file_folder: TP2 :file_folder:

We will create an image that will be a web server that will use the code contained in the folder static-website-example
The goal is that our website is directly embedded in an image.
So when we will deploy our container with our image, our website will be available
We will create 2 images (Dockerfile_v1 and Dockerfile_v2)

The 1st file (Dockerfile_v1) requires to make a copy (with the git clone command) of our static-website-example folder, before building a Docker image
The 2nd file (Dockerfile_v2) does not require this step because it directly integrates the git clone command which allows to retrieve dynamically the last version of the application code which is on Github

## :file_folder: TP5 :file_folder:

The goal of this exercise is to deploy the Odoo application using docker-compose
This file is composed of the "web" service based on the odoo image and the "db" service based on the postgres image
We have add bind mounts and volumes for data persistence
Finally we expose the application on port 80 and we put it in the odoo_network

## :file_folder: Mini-projet :file_folder:

To summarize the objective is to create microservices from the source code that is provided to containerize an application with 2 modules: PHP (front-end) and Flask (back-end).
The goal is to produce the Dockerfile and the docker-compose to deploy this application.
Finally we will deploy a private registry and store the built images.

So in my repository you will find my Dockerfile, my docker-compose file and a cheat sheet which contains all the commands needed to build this project  

You will find below the full statement proposed by Eazy-training : 

# student-list

This repo is a simple application to list student with a webserver (PHP) and API (Flask)

[![project](https://user-images.githubusercontent.com/18481009/84582395-ba230b00-adeb-11ea-9453-22ed1be7e268.jpg)](https://user-images.githubusercontent.com/18481009/84582395-ba230b00-adeb-11ea-9453-22ed1be7e268.jpg)

----------

## [](https://github.com/diranetafen/student-list#objectives)Objectives

The objectives of this practice exam are to ensure that you are able to manage a docker infrastructure, so you will be evaluated about the following

### [](https://github.com/diranetafen/student-list#themes)Themes:

-   improve an existed application deployment process
-   versioning your infrastructure release
-   address best practice when implementing docker infrastructure
-   Infrastructure As Code

## [](https://github.com/diranetafen/student-list#context)Context

_POZOS_  is an IT company located in France and develops software for High School.

The innovation department want to disrupt the existing infrastructure to ensure that

it can be scalable, easily deployed with a maximum of automation.

POZOS wants you to build a  **POC**  to show how docker can help you and how much this technology is efficient.

For this POC, POZOS will give you an application and want you to build a "decouple" infrastructure based on  **Docker**.

Currently, the application is running on a single server with any scalability and any high availability.

When POZOS needs to deploy a new release, every time some goes wrong.

In conclusion, POZOS needs agility on its software farm.

## [](https://github.com/diranetafen/student-list#infrastructure)Infrastructure

For this POC, you will only use one single machine with a docker installed on it.

The build and the deployment will be made on this machine.

POZOS recommends you to use centos7.6 OS because it's the most used in the company.

Please also note that you are authorized to use a virtual machine base on Centos7.6 and not your physical machine.

The security is a very critical aspect of POZOS DSI so please do not disable the firewall or other security mechanisms otherwise please explain your reasons in your delivery.

## [](https://github.com/diranetafen/student-list#application)Application

The application that you will be working on is named "_student_list_", this application is very basic and enables POZOS to show the list of the student with their age.

student_list has two modules:

-   the first module is a REST API (with basic authentication needed) who send the desire list of the student based on JSON file
-   The second module is a web app written in HTML + PHP who enable end-user to get a list of students

Your work is to build one container for each module an make them interact with each other

Application source code can be found  [here](https://github.com/diranetafen/student-list.git "here")

The files that you must provide (in your delivery) are  _**Dockerfile**_  and  _**docker-compose.yml**_  (currently both are empty)

Now it is time to explain you each file's role:

-   docker-compose.yml: to launch the application (API and web app)
-   Dockerfile: the file that will be used to build the API image (details will be given)
-   student_age.json: contain student name with age on JSON format
-   student_age.py: contains the source code of the API in python
-   index.php: PHP page where end-user will be connected to interact with the service to - list students with age. You need to update the following line before running the website container to make  _**api_ip_or_name**_  and  _**port**_  fit your deployment  `$url = 'http://<api_ip_or_name:port>/pozos/api/v1.0/get_student_ages';`

## [](https://github.com/diranetafen/student-list#build-and-test-7-points)Build and test (7 points)

POZOS will give you information to build the API container

-   Base image

To build API image you must use "python:2.7-stretch"

-   Maintainer

Please don't forget to specify the maintainer information

-   Add the source code

You need to copy the source code of the API in the container at the root "/" path

-   Prerequisite

The API is using FLASK engine, here is a list of the package you need to install

```
apt-get update -y && apt-get install python-dev python3-dev libsasl2-dev python-dev libldap2-dev libssl-dev -y
pip install flask==1.1.2 flask_httpauth==4.1.0 flask_simpleldap python-dotenv==0.14.0

```

-   Persistent data (volume)

Create data folder at the root "/" where data will be stored and declare it as a volume

You will use this folder to mount student list

-   API Port

To interact with this API expose 5000 port

-   CMD

When container start, it must run the student_age.py (copied at step 4), so it should be something like

`CMD [ "python", "./student_age.py" ]`

Build your image and try to run it (don't forget to mount  _student_age.json_  file at  _/data/student_age.json_  in the container), check logs and verify that the container is listening and is ready to answer

Run this command to make sure that the API correctly responding (take a screenshot for delivery purpose)

`curl -u toto:python -X GET http://<host IP>:<API exposed port>/pozos/api/v1.0/get_student_ages`

**Congratulation! Now you are ready for the next step (docker-compose.yml)**

## [](https://github.com/diranetafen/student-list#infrastructure-as-code-5-points)Infrastructure As Code (5 points)

After testing your API image, you need to put all together and deploy it, using docker-compose.

The  _**docker-compose.yml**_  file will deploy two services :

-   website: the end-user interface with the following characteristics
    -   image: php:apache - environment: you will provide the USERNAME and PASSWORD to enable the web app to access the API through authentication
    -   volumes: to avoid php:apache image run with the default website, we will bind the website given by POZOS to use. You must have something like  `./website:/var/www/html`
    -   depend on: you need to make sure that the API will start first before the website
    -   port: do not forget to expose the port
-   API: the image builded before should be used with the following specification
    -   image: the name of the image builded previously - volumes: You will mount student_age.json file in /data/student_age.json
    -   port: don't forget to expose the port

Delete your previous created container

Run your docker-compose.yml

Finally, reach your website and click on the bouton "List Student"

**If the list of the student appears, you are successfully dockerizing the POZOS application! Congratulation (make a screenshot)**

## [](https://github.com/diranetafen/student-list#docker-registry-4-points)Docker Registry (4 points)

POZOS need you to deploy a private registry and store the built images

So you need to deploy :

-   a docker  [registry](https://docs.docker.com/registry/ "registry")
-   a web  [interface](https://hub.docker.com/r/joxit/docker-registry-ui/ "interface")  to see the pushed image as a container

Or you can use  [Portus](http://port.us.org/ "Portus")  to run both

Don't forget to push your image on your private registry and show them in your delivery.

## [](https://github.com/diranetafen/student-list#delivery-4-points)Delivery (4 points)

Your delivery must be zip named firstname.zip (replace firstname by your own) that contain:

-   A doc or PDF file with your screenshots and explanations.
-   Configuration files used to realize the graded exercise (docker-compose.yml and Dockerfile).

Your delivery will be evaluated on:

-   Explanations quality
-   Screenshots quality (relevance, visibility)
-   Presentation quality

Send your delivery at  _**[eazytrainingfr@gmail.com](mailto:eazytrainingfr@gmail.com)**_  and we will provide you the link of the solution.

[![good luck](https://user-images.githubusercontent.com/18481009/84582398-cad38100-adeb-11ea-95e3-2a9d4c0d5437.gif)](https://user-images.githubusercontent.com/18481009/84582398-cad38100-adeb-11ea-95e3-2a9d4c0d5437.gif)
