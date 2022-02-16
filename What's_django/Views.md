# Views

When Your're first begining to create websites, having static pages seems only natural

But to produce dynamic content we use _templates_

Templates can ease the developing process of apps

- It gives us reusability of the same code for multiple times _where section of data change depending on the request_

> vies are typically a component tht display information to the user.

## Task of views

vary depending on the framework and conventionsb including responsibility for loading data.

in Django views are responsible for

- Validating a user's request
- Loading or modifying the appropriate data.
- Returning an HTML template with the information to the user

A `URlconf` is a list of pattern to match, the function to call and optionally a name.

## create a View

To create a view The function contain the code to:

- Perform the task that user has requested.
- Return a template with the appropriate data to display to the user

`view` function always take on parameter named `request`, which represent the user request

You can include custom parameters if you are expecting more information from the user in the URL,
such as

- Id of an item

You'll create these argument when creating the route.

## Loading Data

We use Django ORm to load data

For our project we use `<models_name>.objects.all()`

we can get an individual item by typing
`<models_name>.objects.get(pk=1)`

> pk refers to the __primary key__ which can be `id` or any other name you have given to primary key

### 404 errors

Status code 404 refers in web appplication as "not found"

one should return if the request is made for an object that doesn't exist.

Django provide shortcuts for trying to load data

- `get_object_or_404` and `get_list_or_404` load an objects by a primary key, or retur a 404 to the user if an object is not found

- `get_list_or_404`: Performs the same operation as the other, except it take a filter parameter.

## Renndering template

Django template take the HTML and data we provide to create HTML to return to browser the function which help in doing so is `render`

to pass the data to template `render` take a dictionary object `context` it contains key value pairs where key matches the variable name in template.

```python
def shelter_list(request):
    shelters = Shelter.objects.all()
    context = { 'shelters': shelters }
    return render(request, 'shelter_list.html', context)
```

## Register a path

Almost any web framwork uses paths to process user request.

Path convert portion of URL after the name of the domain and before the querystring into function call

eg: a call to `www.catpost.com/post` might call a function `post` where as

a cal to `www.catpost.com/view/` might call a function `view` with parameter `id=1`

to register paths create `URLconf` in django

for eg

```python
    path('', views.index, 'index')
```

It route the traffic where path is not specified or the `www.catpost.com` to a function
in views called `index` and give it name called `index`

we can create virtual function for virtual folders for specific request For example if we wanted to list all `catpost` if someone request __catpost/__

```python
path('catpost', views.catpost_list, 'catpost_list')
```

## URL parameters

parameters such as `ID` or a `name` will change so we won't hard code them to url

In django you can specify a parmeter using special syntax in which you indicate the type of data you're expecting and a name

for eg:
if some one is request to view a cat post with `id`  which is of type `integer`(as primary key are integer)
we provide the name we want to use in our view for the vairable which will be passed as argumen to view function the syntax is `<int:pk>` which uses _type declaration : name_

```python
path('catpost/<int:pk>', views.catpost_detail, name='catpost_detail')
```

so the associated view function will have

```python
def catpost_detail(request, pk):
    pass
```

where `pk` is part of path is passed to `catpost_detail` as parameter, just as if we were calling it like a normal python function.

## Django templates

Templats are text files thta can be used to genereate text-based formats such as HTML or XML Each template contains some static data that's shared across the site, but it can also contain placeholder for dynamic data.

Template contain

- variable
- tags

to control the behavior and what will display as the final page.

## Variables

Variable in a template behaves as they do in any other programming language. We can use them to indicate a value that's evaluated at runtime

we use `{{ }}` to define variable in django

any variable is inside `{{ }}` is evaluated for text content and then placed inside the template
so for example we want to display the name of the cat `{{ cat.post }}`

view passes the variable to template using render function which we'll explore in a later module You can pass value and other data to template, including a QuerySet from the Django ORM.
this help you to display data from database into your application.

## Filter

- way to control how data appear when it's request in a template.
- Because filters are already created, they provide an easy way for you to fomat data without writing special function

for eg:
if you want to print the cat name first letter in capital

```HTML
{{ cat.name | capfirst }}
```

variable on left side of `|` pipe and filter is on the right

## Tags

it can perform

- loops
- create text,
- provide other types of command

for tmeplate engine
to define  a tag suite like in python we use `end` tag as the tags are in template and containe the data provided by them.

we can use `if` and `for` for booleand and iteration

```HTML
{% if catposts %}
    <h1>There are {{ dogs | length }} posts</h1>
{% else %}
    <h1>No posts</h1>
{% endif %}
```

## Template inheritance

A lot of structure and theme is consistent with a website to acheive that django provide a template inheritance where
a `base` template is created with `{% block %}` placeholders which can be used to place children content there.

for eg:

### Parent

```HTML
<html>
<head>
    <link rel="stylesheet" href="site.css">
    <title>{% block title %}Shelter site{% endblock %}</title>
</head>
<body>
    <h1>Shelter site</h1>
    {% block content %}
    {% endblock %}
</body>
</html>
```

### Children

```HTML
{% extends "parent.html" %}

{% block title %}
Welcome to the Shelter site!
{% endblock %}

{% block content %}
Thank you for visiting our site!
{% endblock %}
```

we use extend to `extend` the Parent with child content inside of it.

```HTML
<html>
<head>
    <link rel="stylesheet" href="site.css">
    <title>Welcome to the shelter site</title>
</head>
<body>
    <h1>Shelter site</h1>
    Thank you for visiting our site!
</body>
</html>
````

here placeholder is replaced with child content