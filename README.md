# DevOps-1
My answers for the Docker TD from school. DevOps
# DevOps-1

docker run -d \
  --name tp1-db \
  --network backend-network \
  -e POSTGRES_DB=mydb \
  -e POSTGRES_USER=postgres \
  -e POSTGRES_PASSWORD=postgres \
  postgres:15

My answers for the Docker TD from school. DevOps

## ✅ TD1 - Cat Gif Flask App

This project is inside the `TP1/` folder.  
It shows a random cat GIF using Flask and runs in a Docker container.

#1-1 For which reason is it better to run the container with a flag -e to give the environment variables rather than put them directly in the Dockerfile?

because of security, flexibility. If we use -e, this will not be saved in the image, therefore, the codes/tokens, won't be seen in case someone enters the image.
Also is more flexible, I can reuse the same Dockerfile for different environments, i don't have to rebuild the image for each environment 

(-e is how you pass environment variables to the container.)

#1-2 Why do we need a volume to be attached to our postgres container?

because if anything happens, the container will survive, meaning that the volumne would will have a backup, basically it persists the data even if the container is restarted or deleted. if something happens, the data could be deleted from the container, with the volume this doesn't happen.

#1-3 Document your database container essentials: commands and Dockerfile.

FROM postgres:17.2-alpine

ENV POSTGRES_DB=db \
    POSTGRES_USER=usr \
    POSTGRES_PASSWORD=pwd

COPY docker-entrypoint-initdb.d/ /docker-entrypoint-initdb.d/

# Why do we need a multistage build?
We use the mulstisage build in order to build a java app easier. In this case, we mostly need Maven and JDK, to comiple the Java file, but to actually run it, we only need JRE (Java Runtime Environment). So, the multistage helps us keep our docker image small and celan, to remove tools we don't need to build, avoid unnecessary files and make everything smoothier, and cleaner to deploy. This gives us a smaller image, it's faster, more secure. 



#1-5 Why do we need a reverse proxy?

We need a reverse proxy to hide and protect our backend application, manage traffic, and simplify how users access our services. Basically it helps serve everything from 1 URL, we can add security features as SSL/hhtsp, it serves static files and front-end directly from Apache and finally prevents users from seeing backend port numbers, like the 8080. So, instead of typing localhost:8082, users just go to localhost (or any domain you choose) and Apache redirects traffic to the backend invisibly.

Apache httpd can do more than serve static or dynamic content—it can also act as a reverse proxy server (or gateway). In this setup, Apache does not generate the content itself. Instead, it forwards incoming client requests to one or more backend servers, which handle the request and return the response. Apache then sends that response back to the client.

#1-6 Why is docker-compose so important?

Docker-compose is important because it allows you to define, configure, and run multi-container Docker applications easily using a single YAML file. It simplifies the orchestration of multiple services (like backend, frontend, and database), making it easier to manage dependencies, networking, environment variables, and service startup order.
 
#1-7 Document docker-compose most important commands
Command	Description
docker-compose up: Starts all the services defined in docker-compose.yml
docker-compose up -d: tarts services in detached mode (in the background)
docker-compose down	Stops and removes all services and networks
docker-compose build: Builds images for services defined in the compose file
docker-compose logs:Displays logs from all containers
docker-compose ps: Lists all services defined in the project
docker-compose stop / start / restart: Controls individual or all containers
docker-compose exec <service> <command>	Runs a command inside a running container

#1-8 Document your docker-compose file.

services:
  backend:
    build:
      context: ../simpleapi
    image: simpleapi-app
    container_name: simpleapi-backend
    networks:
      - backend-network
      -create netwok
    depends_on:
      - database

  database:
    image: postgres:15
    container_name: tp1-db
    environment:
      POSTGRES_USER: postgres
      POSTGRES_PASSWORD: postgres
      POSTGRES_DB: students
    volumes:
      - pgdata:/var/lib/postgresql/data
    networks:
      - backend-network

#don't use the psotgree:15 image and 
  httpd:
    image: apache-landingpage
    container_name: apache-server
    build:
      context: ../http-server
    ports:
      - "80:80"
    depends_on:
      - backend
    networks:
      - create network 

networks:
  backend-network:


volumes:
  pgdata:

  the api and proxi will be in the same network 


  #1-9 Document your publication commands and published images in dockerhub.
#1-10 Why do we put our images into an online repo?


define some jobs, 
brach develop create 
develop brach, for new featurs and testing 
main brach, the main one, we create a develop branch so we don't crash it pwd

#2-1 What are testcontainers?

Testcontainers are Java libraries that let you run Docker containers as part of your automated tests. They are super useful when your code depends on external services like databases, message queues, or APIs. if i want to test that your app connects to a PostgreSQL database correctly. Instead of:

Installing a real PostgreSQL database manually on your machine,

Or mocking the database (which might not test everything properly),

You can use Testcontainers to spin up a real PostgreSQL Docker container just for the test and automatically shut it down afterward.

