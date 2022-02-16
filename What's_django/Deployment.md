# Deployment Consederations

An application running in a production environment has different set of needs and requirements than it does in
deployment

Particularly

- Security
- Performance

are not as critical in deployment as in development

Django provide a full check list of predeployment configuration

- Debug mode
  - show error message if error occur during development but can be risk in deployment so
    `setting.py` set 'DEBUG' to `False` before deployment

- Secret Key
  - Django uses key to sing any value that must not be tampered
  - stored in cleartext in `setting.py` `SECRET_KEY` variable update it.

- Static Files
  - this files are not part of django templating system
    suchs as
    - css
    - js
    - images
  - During development django serves any staic files.
    In production you need to configure a servie to serve any static files.
    eg: `Whitenoise`

  - During deployment all static files are collecte into the location indicated by  `STATIC_ROOT` in `setting.py` they are collected using `python manage.py collectstatic`

__Azure__ automatically run the `collectstatic` command so we don't nedd to do locally

## Add libraries

- `whitenoise` to serve static files.
- `psycopg2-binary` to connect POstgresSql the production database

### Creating a deployment setting files

Create a new file inside project name _azure.py_

- `Allowd_hosts` control the server that are allowed to host or run your application.
- `DATABASES` contain list of available connection string.

### Allowed HOSTS

```python
ALLOWED_HOSTS = [os.environ['WEBSITE_HOSTNAME']] if 'WEBSITE_HOSTNAME' in os.environ else []
```

`WEBSITE_HOSTNAME` will be availabel from environment variable

### Databases

```python
hostname = os.environ['DBHOST']
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql',
        'NAME': os.environ['DBNAME'],
        'HOST': hostname + ".postgres.database.azure.com",
        'USER': os.environ['DBUSER'] + "@" + hostname,
        'PASSWORD': os.environ['DBPASS'] 
    }
}
```

The connection string you're using is for PostgreSQL. You provide the following information:

- ENGINE: Database type
- NAME: Name of the database
- HOST: URL for the server
- USER: Username to use to connect to the database
- PASSWORD: Password for the user

### Whitenoise

Add `whitenoise` middleware to serve static files
in deployment
WhiteNoise is middleware that reads user requests for static files such as CSS or JavaScript. It also ensures the files are served correctly. You registered the middleware by updating the MIDDLEWARE array. You registered a `STATIC_ROOT` to store static files.

### SECRET_KEY

```python
SECRET_KEY = os.getenv('SECRET_KEY')
```

get it from environment

### DEBUG

disable it

```python
DEBUG = False
```

## For configure production setting

Look for `WEBSITE_HOSTNAME` environment variable which indicat the application is running on azure

1. open `settings.py`
1. add at the end of the file to override setting.

```python
if 'WEBSITE_HOSTNAME' in os.environ: # Running on Azure
from .azure import *
```

- save this




# Deployment considertaion

we can deploy an application in the cloud we need to determin
- How to deploy
- What database to use

__Azure give two Deployment option__
- Azure Databases
- Azure app service

## Create the database schema 
we can run Sql or `makemigrations` command to make django update the dtaabases directly

to run teh command we need to (SSH) secure-shell into the apllicaton and run command as we run them locally.
