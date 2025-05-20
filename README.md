# DevOps-1
My answers for the Docker TD from school. DevOps
# DevOps-1

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

