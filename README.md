Deploy your own ELK stack using Docker Compose

Introduction
In this tutorial, you will learn how to install an ELK stack using Docker Compose on a server with Ubuntu (version 22.04). The ELK stack is comprised of Elasticsearch, Kibana, and Logstash.

Elasticsearch is a search and analytics engine.
Kibana is a user interface for data analysis.
Logstash can analyse logs from applications.
Prerequisites

A server running Ubuntu version 22.04 or later
SSH access to that server
Access to the root user or a user with sudo permissions
Basic knowledge of Docker, Docker Compose, ElasticSearch and YAML
Example terminology

Username: holu
Hostname: <your_host>
# Step 1 - Install Docker Compose
You may skip this step if you have already installed Docker Compose on your server. First, SSH into your server using the following command:

Replace holu with your own username and <your_host> with the IP of your server.
'''ssh holu@<your_host>'''

Make sure to update apt packages and install cURL:
'''sudo apt-get update && sudo apt-get install curl -y'''

After making sure curl is installed, we can use the quick install script provided by Docker to install Docker as well as Docker Compose:
'''curl https://get.docker.com | sh'''

This command will download the script from get.docker.com and "pipe" it to sh (It will feed the downloaded script to sh which will execute that script and install Docker). 
The last thing we can do is add ourselves to the Docker group so that we don’t need to use sudo everytime we use the docker command.

Replace holu with your own username.
'''sudo usermod -aG docker holu'''

# Step 2 - Create the Docker containers

# Step 2.1 - Create docker-compose.yaml
The docker-compose.yaml file will be used to declare all the infrastructure for the ELK stack. It is used to create several containers with a single command.

Often, containers are not stored in the users directory but in /opt/containers.
'''sudo mkdir -p /opt/containers &&  cd /opt/containers
   sudo chown -R holu /opt/containers'''
   
Create a new folder on your server and create a docker-compose.yaml file and an esdata directory in it:
'''mkdir elk-stack && cd elk-stack && touch docker-compose.yaml && mkdir esdata && sudo chown -R 1000:1000 esdata'''

We will store the elasticsearch data permanently outside the elastissearch container in the esdata directory.
We want to use Docker Compose to create three Docker containers:


