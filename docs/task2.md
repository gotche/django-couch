# Create the necessary scripts for dockerizing it

The easiest way to dockerize a django app is using the official django docker image. It uses onbuild hooks to copy the app, install the requirements and run the development server.

Dockerfile:

```
FROM django:onbuild

```

To build the image:
```
docker build -t djapp .
```

And to run it:

```
docker run --name djapp1 -p 8000:8000 -d djapp
```

This is the easiest way, but usually it is not what you want. For development or as a proof of concept is ok, but I wouldn't run this docker image in production. It is running the development server and this is neither secure nor performant. This django app is a wsgi application, so we are going to use a wsgi server to run it. One of the most famous one is uwsgi, and usually is used with supervisor to control it the number of instances you are running, or restarting it if it dies.

So this is good enough Dockerfile for production

```
FROM python:2.7.11

RUN apt-get update && apt-get -y install supervisor

ADD . /opt/djapp
RUN pip install uwsgi
RUN pip install -r /opt/djapp/requirements.txt
RUN python /opt/djapp/manage.py collectstatic --noinput
CMD supervisord -c /opt/djapp/config/supervisord.conf

```
