# Generic Views

Data-driven application often share smimilar structure.

You have a page for

- list of items
- detail of items
- create item
- delete item
- update item

creating each page become tedious specially as much of HTML is repetitive

to help streamline the creation of data driven application django provide Generic View class

We use them to quickly create pages to display and edit data.
These viws can even generate HTML form for us.

## Use generic view to display data

common opertation to perform are

1. Load the item from the database by ID.
1. if the item isn't found return a 404.
1. if the item is found pass the item to a template for display

the generic view system provides classes to use which contain the code already written for above functionality

## Detailview and Listview

### Detailview

- retrieve item from model 
- passes to template 
- set `context_object_name` as the context object name we use inside teh template

```python
from . import models
from django.views import generic

class CatDetailView(generic.DetailView):
    model = models.Cat
    template_name = 'cat_detail.html'
    context_object_name = 'cat'
```

the path must have a `pk` parameter set to pass the primary key of the item to return in `DetailView`

#### ListView

It is similar to detailview

difference is in design to work with any form of a query that returns multiple items.
As a result you must override
`get_queryset` function.
The function called  by generic view system to reterieve the items from database
which allow you to filter or order data how we like.

```python
from . import models
from django.views import generic

class ShelterListView(generic.ListView):
    template_name = 'shelter_list.html'
    context_object_name = 'shelters'

    def get_queryset(self):
        return models.Shelter.objects.all()from . import models
from django.views import generic

class ShelterListView(generic.ListView):
    template_name = 'shelter_list.html'
    context_object_name = 'shelters'

    def get_queryset(self):
        return models.Shelter.objects.all()
```

Registering the path is saame

```python
path('', ShelterListView.as_view(), name='shelter_list')
```

## generic views to edit data

Like view the code to edit and create data is repetitive therefore django provide

functionality to work with in `generic`

### Creation workflow

1. user send `GET` to request form to create new item
1. The server send form with `csrf token`
1. the user complete the form and select __submit__ which send `POST` request indicate form is completed.
1. server validate `csrf token` ensure no tampering has taken place
1. server validate all the field to ensure it meet the rules. error message is return if it fails.
1. the server attempts tos save the item to the database. if it fails and error message is returned to the user
1. succesfully saving redirect user to success page.

### Forms

creating a form is tedious it need to update form if model change.

As you might have seen we have indicated all the validation and data types our models required which are coupled with html form element to create form

thankfully generic view takes care of creating form for us using.

- CreateView
- DeleteView
- UpdateView

```python
from . import models
from django.views import generic

class DogCreateView(generic.CreateView):
    model = models.Dog
    template_name = 'dog_form.html'
    fields = ['name', 'description', 'shelter']
```

here the `fields` property define the editable list of fields. by using the property we ensure only the list property can be edited by user and nothing else.

#### UpdateView

```python
from . import models
from django.views import generic

class DogUpdateView(generic.CreateView):
    model = models.Dog
    template_name = 'dog_form.html'
    fields = ['name', 'description', 'shelter']
```

after successful creation or updation django redirect to the detail page for the item. it 
take the url by using `get_absolute_url` on the associated model

we implement by returning correct url usng `reverse` and `kwargs` is used to pass the `pk` to the route.

Model:

```python
from django.db import models
# TODO: Import reverse
from django.urls import reverse
class Dog(models.Model):
    # Existing code
    def get_absolute_url(self):
        return reverse('dog_detail', kwargs={"pk": self.pk})

```

##### Delteview

Delete view identify the item to delte using `pk` and unlike `create` and `update` it does not require the `field` as entire item will be deleted
also as now item is created no url will be return to redirect to so `success_url` is used which look up url with `reverse_lazy`

```python
from . import models
from django.views import generic
from django.urls import reverse_lazy

class AuthorDelete(DeleteView):
    model = Author
    success_url = reverse_lazy('author-list')
```

> Here `reverse_lazy` was used because the order in which information is loaded into `Django`

#### Form template for create and update

generic view create form for us all we need to do is set placeholder for form in template
using `{{ form }}` 
inside `<form method="POST">` element

the code can be genrated dynamically as `{{ form.as_p }}` or `{{ form.as_table }}`
and `submit` button

```HTML
<form method="post">{% csrf_token %}
    {{ form.as_p }}
    <button type="submit">Save</button>
</form>
```
