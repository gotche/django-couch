# Follow the necessary steps in order to have it running on AWS Elastic Beanstalk

AWS Elastic Beanstalk is a PAAS basically. It can run python applications, in the same fashion as Heroku or google appengine, but can also run docker containers, which is more flexible and achives one of the goals we set: being able to run outside of aws.

So the strategy I will follow to deploy the test django app to AWS Elastic Beanstalk is to deploy the container as it is, as uwsgi is serving http. 

In a production environment I would put a nginx proxy in front the up. I have seen in the documentation you can do that with an elb extension. I will leave that out of the test.

First of all we go to the amazon console
https://console.aws.amazon.com

and go to elastic beanstalk service

We need to create first an environment:

First question is about the type of the app. It can we a web app or workers app (for background batch jobs). We choose web:

![elb create environment][elb1]

[elb1]: https://github.com/gotche/django-couch/raw/master/images/create_env1.png "choose type of app"

The next screen lets you choose the platform.

![elb create environment][elb2]

[elb2]: https://github.com/gotche/django-couch/raw/master/images/create_env2.png "choose platform"

The environment determines the platform of the applications that elb is going to run. You can run java, go, php among others. We will choose Docker 

I will choose to deploy a sample app, that is provided by amazon, so I can check if everything works as expected and discard that I have introduced any bugs.

I also gave it a name dja-env for the environment and dja for the app. Sorry for the short cryptic names. I thought I would have to repeat the process several times until I could make it run, but it did quite straight forward. So, great.

It takes a bit to start, around 10 minutes. Here you can see the initialization:

![elb create environment][elb3]

[elb3]: https://github.com/gotche/django-couch/raw/master/images/create_env3.png "initialization"

And at the end we get the dashboard

We can see the application running in the url provided by elastic beanstalk:

```
http://dja-env.a62mwvdswi.us-east-1.elasticbeanstalk.com/
```

I can see the aws docker test app, so I would say this process works. I will now upload our django app. It needs to be zipped with the Dockerfile directly in the first directory level

```
zip djapp.zip -r *
```

I have added the file Dockerrun.aws.json. Usually you can set up some parameters. In our case the only interesting is the logging. 

```
{
  "AWSEBDockerrunVersion": "1",
  "Logging": "/tmp/dapp",
  "Image": {
    "Update": "false"
  }
}
```

We upload the zip file (which we need to version):


![upload][elb4]

[elb4]: https://github.com/gotche/django-couch/raw/master/images/upload.png "upload"

and deploy (a version):

![deploy][elb5]

[elb5]: https://github.com/gotche/django-couch/raw/master/images/deploy.png "deploy"

The deployment may take a while also, something between 5 and 10 minutes is normal.

There is a events tab that can be checked during the deployment that is very useful to detect problems in the dockers. If there is any problem deploying a version, the system rolls back to the previous healthy version.


We can see also the latest events and the status of our app in the dashboard

![deploy][elb6]

[elb6]: https://github.com/gotche/django-couch/raw/master/images/dashboard.png "dashboard"

if we check our service url, the django test app is working

```
http://dja-env.a62mwvdswi.us-east-1.elasticbeanstalk.com/
```

So our application is running
