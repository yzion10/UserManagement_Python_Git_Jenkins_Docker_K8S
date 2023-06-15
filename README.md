# Python_Git_Jenkins_Docker_K8S_Project

**Release date: 15/06/2023**

**This project combines 2 processes:**
1. Running a container built from image by the docker engine for testing the flask server that running inside the container.
2. Running a container by Kubernetes (minikube) for testing the flask server that running inside the container.
Running and creating the service is done by the helm package.
The above 2 processes run using the image that found in dockerhub site.
All this for the benefit of running tests for the flask server outside the container
using docker_backend_testing.py, K8S_backend_testing.py

**This project written in:**

python language

Groovy language for pipeline scripts

yaml language for
docker-compose.yml file,
helm package (Chart.yaml, values.yaml, flask-deploy.yaml, flask_service.yaml)

**prerequisites installations before running this project:**

python3, java, jenkins, Docker, Git, kubernetes (minikube)

1. Docker daemon running (Docker Desktop)
2. Minikube running
3. Existing repo on Docker hub
4. Existing credentials user in Jenkins (for Docker HUB connection).

**It combined the following files:**

db_connector.py - This file connect to a remote MySql and implements a couple of methods (INSERT,SELECT,UPDATE,DELETE)

rest_app.py - This file defines routes with rest api for create, read, update, and delete user from/to the remote MySQL.
he start a flask server (localhost) that will run inside a container that create from docker image.

backend_testing.py - testing rest_app.py file

clean_environment.py - This file stop flask servers (localhost) from rest_app.py file.
it protected with error handling in case servers are not responding

templates - contains html file for render_template in rest_app.py

Dockerfile - An image will be built based on what we defined in the Dockerfile

docker-compose.yml - this file will run the container based on what we defined in the docker-compose.yml

docker_backend_testing.py - test the flask server (rest_app.py) that running inside the container was created by the docker image.
the test will running outside the container and will try to send/get/put/delete resuests to the flask that is inside the container.

requirements.txt - A file which include the requirements to install before executing *.py files

K8S_backend_testing.py - test the flask server (rest_app.py) that running inside the container was created by the Kubernetes (minikube).
the test will running outside the container and will try to send/get/put/delete resuests to the flask that is inside the container.

flaskchart folder - an helm package which contains Chart.yaml, values.yaml, flask-deploy.yaml, flask_service.yaml files
that deploy the image repo and run the container on Kubernetes which eventually expose the url for testing the flask server using K8S_backend_testing.py

jenkinsfile - A jenkins pipeline script (in groovy) which connect to git repository and execute the following steps:
1. Checkout Git Repo holding this project
2. Run rest_app.py - start a flask server locally
3. Run backend_testing.py - test flask server locally
4. Run clean_environment.py - down flask server locally
5. Build docker Image locally
6. Push the docker Image To DockerHub
7. Set compose image version – setting the version inside the .env file for docker-compose (according the pipeline build number)
8. Run docker compose - will run in detach mode (run the container based on what we defined in the docker-compose.yml)
9. Test dockerized app - will run the docker_backend_testing.py
10. Clean container - will call docker-compose down and delete local image

*************************************************************************************************************************
A simple batch commands to test this project locally (without pipeline) on cmd. (run it according to this steps):
1. docker build -t yzion10/dockerapp:1 .
2. docker push yzion10/dockerapp:1
3. echo IMAGE_TAG=1 > .env
4. docker-compose -f docker-compose.yml up -d --build
5. python docker_backend_testing.py
6. docker-compose down
7. docker image rmi yzion10/dockerapp:1
*************************************************************************************************************************
