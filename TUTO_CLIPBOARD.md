FROM docker-registry.valid.appli-gestion.nc/ubuntu:14.04
MAINTAINER DTSI-Infras

ADD config/proxy-dtsi /etc/apt/apt.conf.d/

RUN apt-get update

RUN apt-get install -y nginx

RUN echo "\ndaemon off;" >> /etc/nginx/nginx.conf

#RUN echo 'Hello je suis dans mon container' > /usr/share/nginx/html/index.html
#COPY webapp/index.html /usr/share/nginx/html/

VOLUME ["/usr/share/nginx/html/"]

EXPOSE 80

CMD ["nginx"]

----------------------------------------

<!DOCTYPE html>
<html>
<head>
<title>Welcome to nginx!</title>
<style>
    body {
        width: 35em;
        margin: 0 auto;
        font-family: Tahoma, Verdana, Arial, sans-serif;
    }
    h2 {
       color: blue;
    }
</style>
</head>
<body>
<h1>Welcome to nginx!</h1>
<h2>Hello je suis dans mon container</h2>
<p>If you see this page, the nginx web server is successfully installed and
working. Further configuration is required.</p>

<p>For online documentation and support please refer to
<a href="http://nginx.org/">nginx.org</a>.<br/>
Commercial support is available at
<a href="http://nginx.com/">nginx.com</a>.</p>

<p><em>Thank you for using nginx.</em></p>
</body>
</html

----------------------------------------

sudo docker build -t hello-docker .

sudo docker run -d -p 8080:80 --name myFirstHelloDocker hello-docker

sudo docker run -d -p 8080:80 -v ~/tuto-docker/webapp/:/usr/share/nginx/html/ --name myFirstHelloDocker hello-docker

sudo docker rm -f myFirstHelloDocker

--------------------UBUNTU--------------------

FROM docker-registry.valid.appli-gestion.nc/ubuntu:14.04
MAINTAINER DTSI-Infras

ADD config/proxy-dtsi /etc/apt/apt.conf.d/

RUN apt-get update && \
    apt-get install -y nginx && \
    echo "\ndaemon off;" >> /etc/nginx/nginx.conf && \
    rm -rf /var/lib/apt/lists

#RUN echo 'Hello je suis dans mon container' > /usr/share/nginx/html/index.html
#COPY webapp/index.html /usr/share/nginx/html/

VOLUME ["/usr/share/nginx/html/"]

EXPOSE 80

CMD ["nginx"]

---------------------ALPINE-------------------
FROM docker-registry.valid.appli-gestion.nc/alpine:3.3
MAINTAINER DTSI-Infras

ENV http_proxy http://proxy.recif.nc:80

RUN apk add --update nginx && \
    rm -rf /var/cache/apk/*
RUN mkdir -p /tmp/nginx/client-body

VOLUME ["/usr/share/nginx/html/"]

EXPOSE 80

CMD ["nginx", "-g", "daemon off;"]

----------------------------------------
hello-world:
     build: .
     volumes:
       - ~/tuto-docker/webapp/:/usr/share/nginx/html/
     ports:
       - 9080:80

