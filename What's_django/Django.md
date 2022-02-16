# Django

It was named after famour jazz guitarist 'Django Reinhardt'.

It's suitable for both front and backend development

Python web framework,The integrated python libraries makes it easy for rapid development.

## Application types

- Machine learning
- E-commerece platforms
- Data analysis
- content management

### Properties

- Full-stack
- ideal for data driven applications
- potentially more of a learning curve
- out-of-the box security and built in admin panel
- custome HTML Templating engine : Django template engine.

### Basic concept

- Project
  - There will always be one Django project.
  - Project contains necessary settings or apps for the website
  - project cannot be used across other projects  
- App
  - There can be many number of app in a single project
  - It is a component of larger project
  - Apps can be used across projects

### Views

Views are component of Django app

- Serve a specific function within app.

- Contain all the necessary code that will return a specific response when requested such as images, templates or redirect if no logic need to be performed.

### Url mapping

- Django contain url Mapping called `URLConf`. (it's a python module in project and app directory called `urls.py`)
- serve as a __table of content__ for the app.

Whe a url is requested the `URLconf` module:

- find the appropriate link within the project
- redirect the request to views file contained in app.

then the `view` process the request and performs the necessary operation.

- `URLconf`: lists of pattern which map a pattern to a handler (function or Class based view)

it allow to manage all the necessary url. mapping

### Create django project

```mkdir <django_project>```
give the directory name you want for your project

```cd <django_project>```
Change your directory into the project folder you made above.

open the vscode or any other editor in this directory
for vscode use this command to open it in current directory by typing
```code .```

For this project we will be using virtual environment.

what is _virtual environment_?

when we install python it will be install in your __C:__ drive or the custom location where you have installed your python.
this is called your Global python installation path. every program which will use python in your computer will refer to this installation.

So suppose we want to develop a Django project, and for this project we require Djanog version 3.2.1

so when you type
```pip install Django==3.2.1``` to install this specific version this will look into your global python installation python and install django in _Lib_ folder inside Global python intallation path.

Now when you are done with the project and want to create another project with
_Django version 4.0_ we need to install Django again, cause the Django we installed for the previous project is 3.2.1
so when we type ```pip install Django```
for this installation we are not specifying a specific version cause the version 4 is the latest and by default pip install the latest packages.

when we perform this pip will update the Django inside our _Lib_ folder in python Global installation.

this is a problem if you later want to check your first project which required you to install Django==3.2.1 and not 4.0

if you are like me and thinking it's not an issue, if I want to use older version I can install it again using ```pip```

but as you will see this only works if you have small number of projects what will happen if you have large number of project we cannot install 10 or 15 packages for each project and we will not even remember the version of the package required for specific package

and for that we use virtual environment as the name suggest it create a virtual environment where we install and store all the package require by our project without affecting the Global installation.

it is a directory in each project which contain all the project required by that project.

in python we can create a virutal environment by using __venv__ is a buitlin python module

!['python -m venv env'](C:\Users\drkwo\Downloads\django_virtual.png")

In above command python refers to the python global installation in your system and _-m_ switch is used to run a python builtin module venv and last we name the virtual environment directory _env_ in this case

after creating the virtual environmnet we need to activate it
to activate the virtual environment type
```env\Scripts\activate```
where env is the environment directory you just created and activate is a program which is inside ```env\Scripts``` directory

after activating the environment you should be able to see.
![1environment](path)

### requirements

remember that I told you we have to remember each installed package and their version well python ```pip```
takes care of it with something called __requirements.txt__ which is as you can see a text file which contain
packages with their version required by the application

we can create requirements.txt by typing
```pip freeze > requirements.txt```
python will automatically install all the package and their version used by our program into requirements.txt
as of right now our project doesn't contain any package so using ```pip freeze`` command to create _requirements.txt_ will not do any good as we don't have any package installed in our virtual environment.

again ```pip``` is used to not only to freeze the packages required by the application but also install them if you give it a requirements.txt file but how can we do that

- First we need to create a _requirements.txt_ file in our project directory then
- Inside of the requirements.txt file type __Django__ on the first line and save it

> Now your must have a requirements.txt file with a single word inside of it __Django__ on the first line.

then all we have to do is pass the requirements to ```pip``` like this

```pip install -r requirements.txt```
where we say pip to install using -r option it will look into give requirements.txt file and recursively donwload all the package defined there.

in our case it'll install Django package.

### Creating project with Django-admin

when django is installed it provide a command  to work with django called ```django-admin```

to create a project we type

```django-admin startproject <project name> .```

the dot at last state that we want to create project in current directory so django won't create a subdirectory for us automatically.

The structure of Django project is:

```text
manage.py
<project name>/
    __init__.py
    settings.py
    urls.py
    asgi.py
    wsgi.py
```

as you can see at top we have _manage.py_ it is a command line utility with same function as ```django-admin``` python module so to run it.
```python manage.py help```
this will provide you help for the command utility.

#### Project Structure

- init.py: It is an empty python module that tell python to treat the directory in which it is present as a python package.
- setting.py: contain all the configuration and setting of the project
- urls.py contain url within the project. This is the `URLconf`
- asgi.py and wsgi.py serve as entry point for your web server depending on what type of server is deployed.

Your django project is a program and when you host your site you need your website to interact with server as both of them are different internally and may not be written in same language.

The server and django need a way to communicate with each other the wsgi and asgi are two ways the django and server can communicate.

simple if you only know english you can communicate with people who speak english not any other language. for communication to happend both person must know and understand english.

As I said earlier django comes with lot of buit in features one of which is development server
think of it as a  small server which your can use to develop your application.

to run development server type
```python manage.py runserver```

you will see something like
!["development server"](runserver)

This will cause the project to perform system checks and start development server
to stop the server press ```ctrl+C```

to create a application inside project type
```python manage.py staartapp <app_name>```

with this command your app directory inside project directory should look like

```<app_name>/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py
```

### Understand path and Views

views and path(or routes)

- core to any web framework.
- determine what information should be displayed to user and how to display it.

#### Paths?

All applications allow users to execute different methods or functions through certain mechanism this action might be tapping on a button in a mobile or executing a command from the command line.

In web application, user requests are made by

- Navigating to different URLs.
- Typing it in.
- Selecting a link.
- Tapping a button.

A route tells django if user make a request for a particual url, or path, what function to execute.

Eg: Url: 'https://catpost.com/about' might execute a function called about

or  Url: 'https://catpost.com/login' might execute a function called __authenticate__

- paths in django are registered by configuring `urlpatterns`.

- These patterns identify what django should look for in the URL the user is requesting and determine which function should handle the request.

- These patterns are collected into a module Django calls a `URLConf`(urls.py)

#### Views?

- Determine what information should be returned to the user.
- Function or classes that execute code in response to the user request.
- Return HTML or other types of responses such as 404 error.

### Create paths and Views

#### Create View

open `views.py` inside the apps folder
and type

```python
from django.shortcuts import render
from django.http import HttpResponse

def index(request):
    return HttpResponse("Hello, world!")
```

HttpResponse is a helper function it allows us to return text or other primitive type to caller(Browser in this case)

#### Create Route

open ```urls.py``` inside the apps folder and type

```python
from django.urls import path
from . import views

urlpatterns = [
    path('', views.index, name='index'),
]
```

as you can see urlpatterns tuple is where the views and urls are connected or mapped.
we have imported views to urls.py to use it inside urlspattern

### Register our URLconf with project

Our newly created URLconf is inside our `<app_name>` application. Because the project control all user request we need to register ourl URLconf in core urls.py

1. open urls.py inside `<project-name>`
1. You can see doc at the begining explaining how your can register URLconf modules
1. Replace the line `from django.urls import path` with folowing `import` statement to add `include` and `path`

```python
from django.urls import include, path
```

using `include` allow us to import `URLconf` modules, and `path` is used to identify the    root for the URLconf

- inside the list, underneath the line that reads

```python
urlpatterns = [
    path('', include('hello_world.urls')),
]
```

now code below the doc should be

```python
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('', include('hello_world.urls')),
    path('admin/', admin.site.urls),
]
```

Run app by typing

```python
python manage.py runserver
```

open the url: [http://localhost:8000/](http://localhost:8000/)

You should see __Hello, world!__

## Summary

Although there are many framework for the python language the Django Framework has proven iteself a worthy opponent for developing application

learned

- why Django is great for rapid deployments.
- The difference between django and flask
- The types of applications best for django
- How to install Django
- How to create a simple program.
