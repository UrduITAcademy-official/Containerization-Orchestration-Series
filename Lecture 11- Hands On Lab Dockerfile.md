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

### --- LAB 3 -----
```
FROM ubuntu:latest
RUN apt-cache search nginx
RUN apt-get update && apt-get install curl -y && apt-get install -y nginx
CMD ["nginx", "-g", "daemon off;"]
```

Run following 
```
docker build –t mylinux .
docker run –d –p 8082:80 mylinux
docker exec –ti <container_name> bash 
```

### --- LAB 4 -----
```
FROM ubuntu
RUN apt-get update –y && apt-get upgrade –y && apt- get install curl -y && apt-get install -y iputils-ping && apt-get –y install net-tools
COPY . /tmp
ENV dev=ops
```

```
docker build –t bazlinux:1.1 .
docker run –it bazlinux:1.1 bash
--
ping 1.1.1.1
```

## --- LAB 5 Optional-----

```
-- my_Script.py 
from pystrich.datamatrix import DataMatrixEncoder
encoder = DataMatrixEncoder('This is a DataMatrix.')
encoder.save('./datamatrix_test.png')
print(encoder.get_ascii())
```

```
-- Dockerfile
FROM python:3
ADD my_script.py /
RUN pip install pystrich
CMD [ "python", "./my_script.py" ]
```

## ----- LAB 06 ---------

Make a file `app.py`

```
import time
import redis
from flask import Flask

app = Flask(__name__)
cache = redis.Redis(host='redis', port=6379)

def get_hit_count():
    retries = 5
    while True:
        try:
            return cache.incr('hits')
        except redis.exceptions.ConnectionError as exc:
            if retries == 0:
                raise exc
            retries -= 1
            time.sleep(0.5)

@app.route('/')
def hello():
    count = get_hit_count()
    return 'Hello World! I have been seen {} times.\n'.format(count)

```
Create `requirements.txt` as
```
flask
redis
```

```
#Dockerfile
FROM python:3.7-alpine
WORKDIR /code
ENV FLASK_APP=app.py
ENV FLASK_RUN_HOST=0.0.0.0
RUN apk add --no-cache gcc musl-dev linux-headers
COPY requirements.txt requirements.txt
RUN pip install -r requirements.txt
EXPOSE 5000
COPY . .
CMD ["flask", "run"]
```
 
 

