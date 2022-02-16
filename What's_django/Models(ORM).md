# Work with models and data in Django

## Introduction

It's possible to create an application which interact with the database directly.

direct interaction:

- can lead to code that's duplicated and not secure.

This __problem__ led to the introduction of _object-relational mappers (ORMs)_

ORMs seperate database calls from objects

We can use ORMs

1. to design objects that represent your data.

1. can also manage database operations for you.

Dajngo has built-in ORM which is a core component of the framework.

## Django object-relational mapper

working with a relational database:

- requires a differnt mindset than working with objects in an application.
- switching between these two environments can slow the process of creating application.
- Also converting the results of queries from database into data that the application can use requires extra code.

Object-relational mappers or ORMs solve this problem by acting as __middleware__ between an application and the database.

You create objects that model the data, including adding constraint and other forms of metadata. The ORM then

- Manages creating an updating the databases as needed.
- Handles the queries
- Converts (or maps) the request that you make through your objects into the appropriate database calls.

## Overview of the Django ORM

Django was created for data-driven apps, so it's only natural that it would have an integrated ORM.

The Django ORM uses class syntax and inheritance that you're already familiar with

Django is design to be a web framework it can use the structure of the models that you create to automatically generate them create automatically generate HTML and forms. In most situations Django can dynamically create HTML to allow the user to edit data without requiring us to create the form manually. It can even manage the database call for us.

## Models in Django

Models are heart of any ORM.

> Model: is represtations of some piece of data that your application will work with.

This can be a person a product, a category, or any other form of data that your application needs

## Create a  model

In Django a model is any class that inherits a collection of functionality from `jagno.models.db.Model`

It allow you to query the database, create new entries, and save updates.
You can define field, set metadata, and establish relationship between models.

```python
form django.db import models
class Product(models.Model):
    # details
    pass

class Category(models.Model):
    # details
    pass
```

## Add methods

models is a python class as a result so we can write and override methods  `django.models.Model` provides, or the ones inherent to all python objects

One method in particular to highlight is __str__. You usethis method to display an object if no fields are specified.

so let's say

```python
class Product(models.Model):
    name = models.Textfield()

    def __str__(self):
        return self.name
```

## Add fields

fields define the data structure of a model. Fields might include the name of an item, a creation date, or any other piece of data that the model needs to store.

diferent piece of data have

- diferent data types
- validation rules
- other form of metadata

The django ORM contain a rich suit to configure the fields of models to our specification.
ORM is extensible so you can create your own rules.

### Defining the field type

the core piece of metadata for all fields is the type of data that it will store,

 such as string or number
 Fields type matches both database and HTML form
 control type (such as text and checkbox)

#### Types

- CharField: A single line of text.
- TextField: Multiple lines of text.
- BooleanField: A Boolean true/false option.
- DateField: A date.
- TimeField: A time.
- DateTimeField: A date and time.
- URLField: A URL.
- IntegerField: An integer.
- DecimalField: A fixed-precision decimal number.

To add fields to our Product and category calsses

```python
from django.db import models

class Product(models.Model):
    name = models.TextField()
    price = models.DecimalField()
    creation_date = models.DateField()

class Category(models.Model):
    name = models.TextField()
```

## Field options

they are used to add metadata to allow null or blank values or mark a field as unique.

we can set validation option and provide custome message for validation error

- field options map to appropriate setting in teh database.
- The rules will be enforced in any form django generate on your behalf

field option are passed into the function for the field itself. Differnet field might support different options.

common ones are

- `null`
  - Boolean option to allow null values.
        Default is `False`.
- `blank`
  - Boolean option to allow blank values.
        Default is `False`.
- `default`
  - Allows the configuration of a default value if a value for the field is not provided.
  - If you want to set the default value to a database null, set default to None.
- `unique`
  - This field must contain a unique value.
  - Default is `False`.
- `min_length and max_length`
  - Used with string types to identify the minimum and maximum string length.
  - Default is None.
- `min_value and max_value`
  - Used with number types to identify the minimum and maximum values.
- `auto_now and auto_now_add.`
    -Used with date/time types to indicate if the current time should be used.
  - `auto_now` will always set the field to the current time on save, which is useful for last_update fields.
  - `auto_now_add` will set the field to the current time on creation, which is useful for creation_date fields.

> - null is lack of value for name attribute _'null'_
> - blank is empty value such as empty string in name attribute __''__

```python
from django.db import models
class Product(models.Model):
    name = models.TextField(max_length=50, min_length=3, unique=True)
    price = models.DecimalField(min_value=0.99, max_value=1000)
    creation_date = models.DateField(auto_now_add=True)

class Category(models.Model):
    name = models.TextField(max_length=50, min_length=3, unique=True)
```

## Keys and relationships

A standard practice in relational databases is for each row in a table to have a primary key,

- autoincremented integer.
- Django add automatically to all models that is created by adding field name `id`.

Relational database also have relationship between tables.

- A product has a category
- employee has a manager
- car has a manufacturer

Django ORM support all relationships that your might want to create between your models

The most common relationship is 'one-to-many' or __foriegn key__ relationship

- __foriegn key__ : multiples items share a single attribute. Multiple produts are grouped into a single category for example to model this relationship we use `Foreignkey`

Django automatically adds a property to the parent to access to all children called `<child>_set` where `<child>` is the name of the child object

Category will have `product_set`

Foreign key has __mandatory__ field `on_delete` 
with two option

- CASCADE: when we delete category delete all the  child related to it.
- PROTECT:  whe we try to delete category return an error

> In most solution we want to use  ```PROTECT```

```python
form django.db import models
class Product(models.Model):
    name = models.TextField()
    price = models.DecimalField()
    creation_date = DateField(auto_add_now=True)
    category = models.ForeignKey(
        'Category', # the name of the model
        on_delete=models.PROTECT,
    )

class Category(models.Model):
    name = models.TextField()
    # product_set will be automatically created

```

___

By creating a model you can define any essential fields and the behavior of your data

## Create models

1. models.py open it.
1. add python class

```python
# create your models here
Class Shelter(models.Model):
    name = models.CharField(max_length=200)
    location = models.CharField(max_length=200
    
    def __str__(self):
        return self.name

class Dog(models.Model):
    shelter = models.ForeightKey(Shelter, on_delete=models.PROTECT)
    name = models.CharField(max_length=200)
    description = models.TextField()
    intake_date = models.DateTimeField(auto_now_add=True)

    def __str__(self):
        return self.name
```

By adding model we now have a representation for shelters and dogs note the relationship between `Dog` and `Shelter` class can house many `Dog` values

`auto_now_add` value for `intake_date`.
it automatically sets the field to the current date if a custom date isn't provided.

We're also using `FroignKey` in the `Dog` class.

it define relationship between dog and shelter
by telling django that each dog is related to a single shelter

## Registering the models

All application must be register with the project

- just because an applications is inside project folder doesn't mean it automatically gets loaded

to register find `app.py` in `<app_name>` folder

and add it to `INSALLED_APPS` in settings.py

> Django ORM goes beyond allowing you to interact with data. You can also use it to create and update the database itself throught a process known as migrations.

## Migrations

- A collection of updates to be performed on database's schema.

__Database schema__ is the definition of the database itself, including all tables and columns and relationships between those tables

When we created our models and defined the fields we also defined the tables, columns and realtionship betwen those tables _We created our schema definition by creating our model_

Migrations are used to create and update the database as our models change. as software are constantly changing. How we define our models today might  be different from how we define them tomorrow. Migrations abstract the process of updating the database away from us.

We can then make change to our models and use django to perform necessary changes to database.

to make migrations run `python manage.py makemigrations`


## Display the Sql for the migration

Any operation inside relational database require appropriate SQL when they're run Although you can use the migration tools to update your database directly . Some environment have DB Admin

to build appropriate SQL run
`python manage.py sqlmigrate <app_name> <migrations_name>`


## Display the list of migration
if you want to see the list of migration then
`python manage.py showmigrations`

## Perform migration
The `migrate` command runs a specific migration or all migrations on the database configured in setting.py in the root of your project directory.

`python manage.py migrate <app_name> <migration_name>`

> The `app_label` and `migration_name` parts are optional. If you don't provide either, all migrations will run.

Django include various models of it's table for

- user management system
- managing session
- other internal uses

___

## Work with data

By creating our models, we created and API that we can use  to access the data in our database. 

This API allow us to:

- create,
- retrieve,
- update,
- delete

objects in our database

> we can interact with api using shell provide by django
which include the django project path

`python manage.py shell`

## Create and modify objects

Because they inherit from `Django.models.Model` they inherit the functionality fro the Django  ORM that functionality includes `save` whcih we use to save the object to the datbase.

create a new shelter :

```python
shelter = Shelter(name='Demo shelter', location='Seattle, Wa')
shelter.save()
```

save part will write the object to database as it was created for first time `INSERT` statement will be executed.

but when attribute are changed

```python
shelter.location = 'Air'
shelter.save()
```

it will issue `UPDATE`  statement to update the value

## Retrieving Objects

To retrieve the objects from a database Django provides an `objects` property on all `Model` classes.

the `objects` property provides multiple functions including `all`, `filter`,and `get`

```python
Shelter.dog_set.all()
```

```python
Dog.objects.get(pk=1)
```

like get `filter` allow us to query in the parameter by using `__` to go from perorperty to property. to find all the dog in shelter with name `name` we use
`shelter__name=name`
