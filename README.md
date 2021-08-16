Steps tp deploy React application on GKE


Create docker file

Build image from dockerfile ( command "docker build -t react-docker-gke .")

Test docker image locally (command "docker run -d -it  -p 80:80/tcp --name react-docker-gke react-docker-gke:
latest")

use http://localhost:80 in browser or postman and vrify if it webpage is loaded

Create a new project in CGP and enable billing. (https://console.cloud.google.com/home/dashboard)

Enable Container registy API in your GCP project.

Login to gcloud cell . command "gcloud auth login"

Set the correct project. in cmd navigate to root directory of your project and set the GCP project

command "gcloud config set project react-docker-gke" -->gcloud config set project

Push the docker image to container Registry

To push the Docker image to the Google Container Registry, we have to tag it according to the following pattern reginalEcrHostname/projectId/dockerImageName:tag

command "docker tag react-docker-gke asia.gcr.io/react-docker-gke/react-docker-gke"

verify if the new image with tagged name is created

command to see all the images "docker images"

Now Push the docker image to conatiner registry

command "docker push asia.gcr.io/react-docker-gke/react-docker-gke"

Create Kubernetes cluster in CGP

To create the cluster first you need to enable Kubernetes engine Api from GCP dashboard.

Then create a cluster

command "gcloud container clusters create react-docker-gke-cluster --num-nodes=3 --region=asia-southeast1-a" cluster name nodes and region can be chnaged as per need

configure the cluster for you project

comamnd "gcloud config set container/cluster react-docker-gke-cluster"

Deploy your image to created cluster

command "kubectl run react-docker-gke --image=asia.gcr.io/react-docker-gke/react-docker-gke --port 8080"

check pods

command "kubectl get pods"

Expose your pod

command "kubectl expose pod react-docker-gke --type=LoadBalancer --port 80 --target-port 8080"

Check services

command "kubectl get service"

Wait for external API to show up for your service

Use external ip with your enpoint to test application in Browser or postman.