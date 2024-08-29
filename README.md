# Tech-Writer-assignment
Test assignment for the technical writer position
Test Assignment - Technical Writer

This guide will show you how to prepare an application image and working infrastructure to deploy the image. 
For this purpose, Nordlys is using Kubernetes. 
Kubernetes is a software platform for automating upload deployment, scaling, and application management in containers. To learn more about Kuberentes please visit their web page.

To begin the process first you must install docker. Docker automatically builds images by reading the instructions from a Dockerfile. To find out more about Docker please visit their web page.
After you have completed the installation of the Docker, proceed with the deployment steps.
The application is run in a container in the Kubernetes cluster. To illustrate how it works, let’s use an ExampleApp. The request can be sent to the 8080 port.

First step is to create a directory with the subfolders. Use these commands to create the folder structure:

mkdir quickstart_docker
mkdir quickstart_docker/application
mkdir quickstart_docker/docker
mkdir quickstart_docker/docker/application

The structure should look like the example below:

    quickstart_docker/

      ├──application/

      └──docker/

         └──application/

The quickstart_docker is a catalog of the entire project, and located here are the application and docker subfolders. Inside the application folder is the code for the entire application, and in the docker folder are docker related files and folders. Docker has a subfolder, named application, and located here is the dockerfile for the application. 

It is not good practice to put all files and folders in the root folder!
Note that there are two folders with the same name, but their paths are different, as well as the information they contain.

To deploy the application, it will need its own environment. That requires building an environment with an operating system, python and its dependencies. Until such time we will use Docker Hub to continue with the deployment. 
Add a file in the quickstart_docker/application catalog, named application.py: The file should contain the following lines:

=================================================================

import http.server
import socketserver

PORT = 8000
Handler = http.server.SimpleHTTPRequestHandler

httpd = socketserver.TCPServer(("", PORT), Handler)

print("serving at port", PORT)
httpd.serve_forever()

=================================================================



In the quickstart_docker/docker/application folder add a file with the name Dockerfile. Once the file is created, add the following lines to file: (note the line with the # is a comment to explain the purpose of the line below):

================================================================================

# This uses base image from the registry
FROM python:3.5

# This sets the working directory to /app
WORKDIR /app

# This copies the 'application' directory contents into the container at /app
COPY ./application /app

# This makes the port 8000 available outside this container
EXPOSE 8000

# This executes 'python /app/application.py' when container launches
CMD ["python", "/app/application.py"]

================================================================================



Next step is to open the terminal and type in the following command: 

docker build . -f-docker/application/Dockerfile -t exampleapp

Where:
- docker build - working catalog, build context (location to look for the file)  
-f docker/application/Dockerfile - docker-file; 
-t exampleapp - you tag the image and find it easily later.


Finally, the prompt will list all the images with the relevant information, as seen below:

=========================================================================================

$ docker images

REPOSITORY             TAG             IMAGE ID            CREATED             SIZE
exampleapp             latest          83ioe0edc28a        2 seconds ago       154MB
python                 3.6             05stv8636w3f        6 weeks ago         154MB

=========================================================================================



Once the results are retrieved, put the image to the repository.


Further reading:

Docker: https://www.docker.com/ 
Docker images:  https://docs.docker.com/engine/reference/builder/
Kubernetes: https://kubernetes.io/ 








