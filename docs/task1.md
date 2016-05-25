
# Create a test django app


I create a directory to contain everything

`mkdir django_couch`

I will create a virtualenv to contain all the python dependencies

`pyenv virtualenv django`

and activate it under the current directory:

`echo django > .python-version`

install django

`pip install django`

let's keep the dependencies in the requirements.txt

`pip freeze > requirements.txt`

and let's create the scaffolding for the django project:

`django-admin startproject djapp`


django is an "all batteries included" framework, so it comes with user models and an admin interface so you can already manage objects. We are not going to use any of this features, but we have to create this initial models:

`python manage.py migrate`


At this point we can check already if the project runs:

`python manage.py runserver`

and we check the browser at localhost:8000

![django installation][django1]

[django1]: https://github.com/gotche/django-couch/raw/master/images/django1.png "django installation succeeded"

