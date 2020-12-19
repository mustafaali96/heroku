#Deploy Django app to heroku 

> mkdir deployments

> cd deployments

> mkdir heroku

> cd heroku

> python -m venv venv

> cd venv\Scripts\

> activate

> pip install django django-heroku gunicorn

> pip freeze > requirements.txt

> django-admin startproject mysite .

> python manage.py runserver

url > 127.0.0.1:8000 (check if app is working)

# Heroku 

Create heroku account > create app > app_name

install heroku

> heroku login

# Creating a Procfile
create Procfile with no extension at project root directory

write> web: gunicorn mysite.wsgi

## Updating the settings.py

file mysite/settings.py

import django_heroku # < here

import os

DEBUG = False # < here

ALLOWED_HOSTS = ['app_name.herokuapp.com'] # < here

#at the bottom of file

django_heroku.settings(locals())

try:
    from .local_settings import *

except ImportError:
    pass


# Create local_settings file in mysite

DEBUG = True

ALLOWED_HOSTS = []


# Create git repo

Create a .gitignore file in the site root:

venv
local_settings.py
db.sqlite3
*.pyc
__pycache__/
*.py[cod]
.DS_Store

## manually create repo and connect it to heroku app

> git init

> git add .

> git commit -m "Initial"

> heroku git:remote -a app_name

> git push heroku master


> heroku run python manage.py migrate (make sure heroku is loggin)

> heroku run python manage.py createsuperuser


#App

> django-admin startapp blog

blog/views.py 

from django.shortcuts import render

def index(request): # < here

    return render(request, 'blog/index.html')

Create index.html

blog/templates/blog/index.html


{% load static %}
<!DOCTYPE html>
<html lang="en">
<head>
<title>Blog</title>
<link rel="stylesheet" href="{% static 'blog/css/site.c\
ss' %}">
</head>
<body>
<div id="content">
<h1>Home</h1>
</div>
</body>
</html>


# Create site.css file

blog/static/blog/css/site.css
 
h1 { color: red;}


# Add path

mysite/urls.py

path('', views.index, name='index')

# Add blog to Installed_Apps

Installed_Apps = [.... , 'blog']