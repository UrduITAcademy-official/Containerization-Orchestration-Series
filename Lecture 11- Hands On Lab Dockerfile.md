# Lecture 11 - Hands On Lab Dockerfile

### --- LAB 1 -----

```
FROM nginx:latest
MAINTAINER baz
COPY . /usr/share/nginx/html
ENV usernmame1=devopsguy # comment 
EXPOSE 80
```
Run following commands 
```
docker build -t webapp1 .
docker images 
docker run -d –p 8081:80 webapp1
```


### --- LAB 2 -----
//  Same Dockerfile
// Make a repo on Docker Hub as myapp
```
docker login --username zeeshanmunir18
docker build -t bazapp .
docker tag bazapp zeeshanmunir18/myapp
docker push zeeshanmunir18/myapp
```


 
