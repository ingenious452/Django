# Introduction to the django admin site

When we create an application we want to create individual permission fo site access creating an admin site interface for employees or clients to manipulate content can be cumbersome

Luckiley for us `Django'` comes with it's own admin site.


## Permission of admin site

Dajngo includes built-in admin site that can be used to manage the data in your application and security.


## Security and Dajango

Django is designed to streamline the creation of data-driven web applications by providing the normal functionality that's required by these types of sites.

Django provide 
- authentication
- authorization
and we can expand it as required

Django has three main type of users `defualt: users, staff and superuser` we can create our own my making group or setting unique permissions

### Access levels
|Access|User|Staff|Superuser|
|------|:----:|:------:|:--------:|
|Admin site| No|Yes|Yes|
|Manage data|No|No|Yes|
|Manage users|No|No|Yes|


## To create users

we first must create a `createsuperuser`  from `manage.py`
then we can acess the admin site to create other users.


Through admin site:

- determin which user has acess to what data
- Users can use the admin site to add, update, delete items.

they don't need to access the database directly.
all we have to do is `activate` our models to appear on admin site.

___

## Register models

open your app/admin.py

```python
# Register your models here.
from .models import Model1, Model2

admin.site.register(Model1)
admin.site.register(Model2)
```

> The object information on admin site appears because we set the __str__ method on our objects. The default display of any object is the value returned by __str__.
