# What would be your alternative proposal for deploying on AWS an application like the one described on step 4?

Due to the limitation of being able to run in a different cloud provider, I would stick to docker to run the app. 

Now, I would install couch db using an ec2 instance, and getting a turnkey image from the marketplace. There is no fee, just a little bit of setup. You need to ssh in the machine using the root account, once it has been launched and follow the steps (the initialization program starts automatically)

You also need to setup security groups so couchdb is accessible from the environment where our app is running in elastic beanstalk.

This setup can be easily migrated to google cloud platform, using google's container engine for the docker apps plus google computer engine for the couchdb instance. And also to a classical VPS provider, using kubernettes for the dockers and virtual machines for the couchdb instance.
