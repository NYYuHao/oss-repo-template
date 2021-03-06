# Example 00

Scary Whale with `docker run docker/whalesay cowsay boo`:

![Scary Whale](./images/whalesay.png)

# Example 01

Running `docker run -it ubuntu bash` to run a container with Ubuntu as the image:

![Ubuntu container](./images/Ubuntu-container.png)

Installing and using vim to create a testfile in /root:

![vim testfile](./images/vim-testfile.png)

Scary Cow with `cowsay`:

![Scary Cow](./images/cowsay.png)

# Example 02

`docker ps` after running both containers

![docker ps](./images/docker-ps.png)

RocketChat running:

![Rocket Chat](./images/rocket-chat.png)

# Example 03

Dockerfile:
```
# Comments in Dockerfiles
FROM python:3.5

# Update and install dependencies
RUN apt-get update
RUN pip install Flask

# Add code
ADD . /opt/webapp/

# Set the working directory
WORKDIR /opt/webapp

# Set environment variables
ENV FLASK_APP=hello.py

# Expose the application's port
EXPOSE 5000

# Run the application
CMD ["flask", "run", "--host=0.0.0.0"]
```

Successful Dockerfile build using `docker build -t helloworld .`

![Successful Build](./images/successful-docker-build.png)

Hello world at `localhost:5000` after running with `docker run -p 5000:5000 helloworld`

![Hello world](./images/hello-rcos.png)

# Example 04

Successfully building message-app using `docker build -t message-app .` and
list of images (some remaining from example 03):

![Images After Build](./images/images-after-build.png)

When attempting to run, there is an error regarding mongodb

![Error](./images/cannot-connect-mongodb.png)

docker-compose.yml:

```
version: '3'
services:
  mongo:
    image: mongo:4.0.7
    volumes:
      - mongo-data:/data/db
    expose:
      - "27017"
  app:
    build: .
    ports:
            - "1337:1337"
    links:
      - mongo
    depends_on:
      - mongo
    environment:
      - MONGO_URL=mongodb://mongo/messageApp
volumes:
  mongo-data:
```

Successful messageapp build:

![Successful build](./images/successful-messageapp-build.png)

Successful messageup up:

![Successful up](./images/successful-up.png)

Successful curl messages (prior to update and delete)

![Successful curl](./images/successful-curl.png)

Successful curl after update on hola and delete on hello

![After update / Delete](./images/curl-after.png)
