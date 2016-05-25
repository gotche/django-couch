FROM python:2.7.11

RUN apt-get update && apt-get -y install supervisor

ADD . /opt/djapp
RUN pip install uwsgi
RUN pip install -r /opt/djapp/requirements.txt
RUN python /opt/djapp/manage.py collectstatic --noinput
CMD supervisord -c /opt/djapp/config/supervisord.conf

