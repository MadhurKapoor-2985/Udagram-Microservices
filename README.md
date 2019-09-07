Docker Images

The docker images can be downloaded from below
https://cloud.docker.com/u/closedmoon/repository/docker/closedmoon/udacity-restapi-reverseproxy

https://cloud.docker.com/u/closedmoon/repository/docker/closedmoon/udacity-frontend

https://cloud.docker.com/u/closedmoon/repository/docker/closedmoon/udacity-restapi-user

https://cloud.docker.com/u/closedmoon/repository/docker/closedmoon/udacity-restapi-feed

Starting The Docker App on a local container

1) Download the images from the above location

2) Configure the following variables in your bash profile or Path . 
POSTGRESS_USERNAME, POSTGRESS_PASSWORD,  POSTGRESS_DB,  POSTGRESS_HOST, AWS_BUCKET, AWS_PROFILE, AWS_REGION,  JWT_SECRET, URL

Set there value as per the RDS, AWS Profile, S3 bucket setting for your AWS account

3)Clone this Repo. Navigate to "\Udagram-Microservices\course-03\exercises\udacity-c3-deployment\docker"

4) Run "docker-compose up". THis will start server and the front end can be accessed from http://localhost:8100 which talks to the feed and user service

Start App on Kubernetes Cluster

I have installed Kubernetes on my AWS account using Kubeone.

Follow the instructions below to set it up  

https://github.com/kubermatic/kubeone/blob/master/docs/quickstart-aws.md

The User service, Feed service, frontend and reverseproxy are running as separate services in POD. 

1) Navigate to "\Udagram-Microservices\course-03\exercises\udacity-c3-deployment\k8s"

2) In the "env-secret.yaml" file, enter your Postgress Username and password as base64 encoded string

3) In the "env-configmap.yaml" file, enter the parameter values

4) Set the aws cred file in "aws-secret.yaml" as base 64 encoded

5)Run below command to add config map and secret
"kubectl apply -f env-configmap.yaml", "kubectl apply -f env-secret.yaml",  "kubectl apply -f aws-secret.yaml"

6) Run below commands for service deployments
kubectl apply -f backend-user-deployment.yaml
kubectl apply -f backend-feed-deployment.yaml
kubectl apply -f frontend-deployment.yaml
kubectl apply -f reverseproxy-deployment.yaml

7) Verify that the deployment is successful by running
kubectl get deployment
kubectl get pods

8) Make sure all the pods are running

9) Use below command to forward the ports
kubectl port-forward service/frontend 8100:8100 kubectl port-forward service/reverseproxy 8080:8080

10) Access the application using http://localhost:8100

Travis CI

Integration is done with Travis CI for continuos build and deployment. Please see the screenshot

Deploying new version

If a new updated image has to be deployed, then update the name to "name:updversion". Update the yaml file to point to the new image. Redeploy the image using kubectl apply -f deployment file

Cloudwatch is enabled for EC2 instance where the cluster is running and provides all the information about the CPU utilization etc.

Screenshots

The screenshots are uploaded in the repo showing the Pods, Travis CI build and app running on kubernetes





