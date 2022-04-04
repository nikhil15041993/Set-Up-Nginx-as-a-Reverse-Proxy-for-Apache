## 1. Create nginx conf file

```
sudo vi nginx.conf
```
Add the following proxy-related options to your Nginx configuration file.

```
server {
    listen 80 ;
    location / {
        proxy_pass http://<service name>;
    }
}
```

## 2. Create Dockerfile

```
sudo vi Dockerfile
```

```
FROM nginx
RUN rm /etc/nginx/conf.d/*
COPY nginx.conf /etc/nginx/conf.d/
```

## 3. Create a docker compose yml file 

```
file.yml


version: '3.3'

services:
  myapp:
    image: httpd


  myproxy:
    image: myproxy
    container_name: myproxy-container
    ports:
    - 80:80 
          
```
Run docker compose file
```
docker-compose -f file.yml up  -d
```

## 4. Scale the service 

We can scal the service in docker comopse by using the following command

```
docker-compose -f file.yml up  -d --scale myapp=2
```
