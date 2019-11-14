This is a project for Udacity Devops Engineering program. 

The project runs a Flask App using Gunicorn. App is deployed as a docker container to the kubernetes Cluster. Deployment is done through Jenkins.

eks: folder contains cloudformation scripts to deploy a kubernetes cluster. Within EKS folder there are 3 folders.

eks/VPC: This folder contains cloud formation script to deploy the VPC,private and public subnets.

eks/EKS: This folder contains cloudformation script to deploy the Amazon EKS cluster.

eks/nodes: This folder contains cloudformation script to deploy the worker nodes with appropriate security groups

eks/aws-auth-cm.yaml: This file contains rolearn that connected worker nodes to out cluster.

Docker folder contains the docker file and a flask app scripts and it's template

Docker/Dockerfile: Docker file

Docker/server.py: Script running the flask app.

Docker/requirement.txt: Requirement packages to get our app running successfully

Docker/templates: This folder has index.html that is referenced in server.py file

deploy.yml: This file applies our flask app to our kubernetes cluster.

flask-service.yml: This file creates an classic load balancer for our cluster.

Jenkinsfile: This file contains our pipeline to deploy our app to the cluster. Including pushing image registry to docker hub and pulling it and deploying it on our cluster. 

AnsibleJenkins folder has a ansible script to deploy a EC2 instance. It does not actually install jenkins services as of now.


