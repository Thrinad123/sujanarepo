Introduction --
Creating a Private Cluster Using Terraform Code 
Create VPC and Subnets
Creating a service account, artifact registry, firewalls, nat-gateway, router using terraform 
giving artifact registry permissions

## Change the Project name in variables.tf near variables {project = "Your Project Name"}

## Create a terraform.tfvars file and add authorized_source_ranges =  [""] and add your internal IP address so that you can access your cluster with CIDRblock "Your-IPADDRESS"/32

## create a Docker file and push the image to Artifact registry
1) make sure to start your Docker service --- sudo service docker start
2) get the website for which you want to create an image using this command
wget https://www.free-css.com/assets/files/free-css-templates/download/page275/aj.zip
3)make sure your docker file is within the folder where the website has been extracted
4)DockerFile looks like this if you are using an apache
FROM httpd
COPY . /usr/local/apache2/htdocs/
5)building the docker image ---- docker build -t image-name .
6) docker run -itd -p 80:80 --name image-secname image-name
7)docker build -t europe-west2-docker.pkg.dev/cicdsujana-345110/my-repository/testwebsitegold:ver1 .   ## we are giving the location
8)docker push europe-west2-docker.pkg.dev/cicdsujana-345110/my-repository/testwebsitegold:ver

## once the image is pushed to artifact registry
1) make sure you connect to connect your cluster using cluster credentials
2)see if your cluster is working or not by using the command 
kubectl get nodes
3)Once you know your cluster is working you can create deployments and expose it

## added deployment.yaml
in the deployment.yaml file you have to change the image address which looks like this:europe-west2-docker.pkg.dev/cicdsujana-345110/my-repository/testwebsitegold:ver1

use the command-   Kubectl apply -f .  ## all yaml files in that folder will get executed 

## if you want to create individual deployments you can use the following commands
kubectl create deployment deplosujana --image=europe-west2-docker.pkg.dev/cicdsujana-345110/my-repository/testwebsitegold:ver1

## creating a service for that deployment 
kubectl expose deployment deplosujana  --type LoadBalancer --port 80 --target-port 80

## get deployments and services using these comands
kubectl get deployments
kubectl get service

Once we expose the service we get the External IP address and here we can check whether our service or image has been correctly expose or not







