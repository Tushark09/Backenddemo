
# Assignment Nginx, Docker, NodeJs

A sample Nodejs code is given which is having two databases, Mongodb
and Mysql and working CRUD operations for each database using REST
APIs.

## Get Started

Clone The Git Repository.




## 🔗 Links
(https://github.com/Tushark09/Backenddemo.git)





## Dockerfile

1. create Dockerfile for Nodejs

```bash
FROM node:16.14.0-alpine
RUN mkdir -p /usr/src/app
WORKDIR /usr/src/app

COPY package.json /usr/src/app/
RUN npm install mongodb@2.2 --save
RUN npm install

COPY . /usr/src/app
EXPOSE 51005
CMD ["node", "index.js"]


```

## docker-compose.yml

```bash
version: '3.2'
services:
  backend_demo:
    build: .
    depends_on:
      - mongo
    environment:
      PORT: '51005'
      MONGO_DB_URI: 'mongodb://mongo:27017/backend_demo'
    healthcheck:
      test: ["CMD", "curl", "-f", "http://localhost:51005/api/status"]
      interval: 10m
      timeout: 10s
      retries: 3
    ports: 
      - 51005:51005
    restart: on-failure
  mongo:
    image: mongo:latest
    expose:
      - 27017
    restart: on-failure
    volumes:
      - data:/data/db
    
volumes:
  data:

```

##CMD 

```bash
docker-compose up
```

##OUTPUT

check in the browser http://localhost:51005/documentation

Add a custom configuration to Nginx ,Enable HTTPS and redirect HTTP HTTPS


Install Nginx and edit nginx.conf file

```bash


    server {
        listen       81;
        server_name  localhost;
    
        location / {
            proxy_pass   http://localhost:5110
        }

    # HTTPS server
    #
    server {
        listen       443 ssl;
        server_name  localhost;

        ssl_certificate      example.crt;
        ssl_certificate_key  example.key;

        ssl_session_cache    shared:SSL:1m;
        ssl_session_timeout  5m;

        ssl_ciphers  HIGH:!aNULL:!MD5;
        ssl_prefer_server_ciphers  on;

        }
    }
}

```

I have used Oenssl to generate SSL self sign certificates

```bash
openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout privateKey.key -out
certificate.crt
```


