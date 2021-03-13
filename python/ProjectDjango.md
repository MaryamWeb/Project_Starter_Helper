# Django

### Create a virtual environment:
* In terminal type `py -m venv ENV_NAME`
* Activate the virtual environment `call ENV_NAME\Scripts\activate`
* With your virtual environment activated In the terminal run `pip install Django`   
* Make sure we have successfully installed Django by checking its version. In terminal run `python -m django --version`

### Setup
* Create an empty folder `mkdir superheroes` -->  `cd superheroes`
* In terminal run `django-admin startproject superheroes` to create a new Django project
* Navigate into that folder `cd superheroes`
* Create an App `python manage.py startapp hero_app`  

#### Settings.py
```python
INSTALLED_APPS = [
       'hero_app', # Our App
       'django.contrib.admin',
       'django.contrib.auth',
       'django.contrib.contenttypes',
       'django.contrib.sessions',
       'django.contrib.messages',
       'django.contrib.staticfiles',
   ]    
```
* In terminal run `pip install python-decouple`
* Create `.env` file and add the SECRETE_KEY from the seetings.py file `DJANGO_SECRET_KEY = example`
* In settings.py
 ```python
 from decouple import config
 
 SECRET_KEY = config("DJANGO_SECRET_KEY")
```
* Create `.gitignore` file and add 
```
 ENV_NAME/
__pycache__/
*.pyc
.env
```
#### urls.py:
```python
    from django.urls import path, include           
    # from django.contrib import admin              
    urlpatterns = [
        path('', include('hero_app.urls')),	   
        # path('admin/', admin.sites.urls)         
    ] 
```
#### App urls.py:
Next, create a new urls.py file in the hero_app folder.
```python
from django.urls import path     
from . import views
urlpatterns = [
    path('', views.index),	   
]
```
#### App views.py:
```python
from django.shortcuts import render, HttpResponse
def index(request):
    return HttpResponse("Home")
```
Run the application by running the following command `python manage.py runserver 7000`

Check our server at localhost:7000/

### Templates:
* Create an empty folder while in the app directory `mkdir templates`  
* Inside the folder create an html template `touch index.html`
```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Heroes</title>
        <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.3/dist/css/bootstrap.min.css" integrity="sha384-TX8t27EcRE3e/ihU7zmQxVncDAy5uIKz4rEkgIXeMed4M0jlfIDPvg6uqKI2xXr2" crossorigin="anonymous"> 
    </head>
    <body>
        <h1>Welcome Page</h1>
    </body>
 </html> 
```
We can then render templates in our views.py file like so:

```python
from django.shortcuts import render	 
def index(request):
    return render(request, "index.html")
```
To pass data to the template

```python   
from django.shortcuts import render
def index(request):
    context = {
    	"name": "Hulk",
    	"powers": ["strength", "thunderclap", "leaping"]
    }
    return render(request, "index.html", context)
```
```html
<h1>Superhero:</h1>
<p>Name: {{name}}</p>
<p>Powers</p>
<ul>
{% for p in powers %}
   <li>{{p}}</li>
{% endfor %}
</ul>
```
### Static Files:
* create a directory called static
* in static create folders css, img, js
* In our templates, when we want to reference our static files, we'll first need to add a line indicating we want to use our static files:
```html
<!DOCTYPE html>
  <html>
    <head>
      <meta charset="utf-8">
      <title>Index</title>
      {% load static %}		<!-- added this line -->
```
* We can then reference any static files:
```html
<!DOCTYPE html>
  <html>
    <head>
      <meta charset="utf-8">
      <title>Index</title>
      {% load static %}
      <link rel="stylesheet" href="{% static 'css/style.css' %}">    
      <script src="{% static 'js/script.js' %}"></script>
    </head>
    <body>
    	<img src="{% static 'images/image.jpg' %}" />
    </body>
```
### Models:
* Open the file `hero_app/models.py`

```python 
from django.db import models
  
class Hero(models.Model):
    name = models.CharField(max_length=40)
    desc = models.TextField()
    created_at = models.DateTimeField(auto_now_add=True)
    updated_at = models.DateTimeField(auto_now=True)

    def __repr__(self):
        return f'<Hero object: ID:{self.id} Name:{self.name} Desc:{self.desc}>'

```
https://docs.djangoproject.com/en/3.1/ref/models/fields/

### Migrations:
* In the terminal run `python manage.py makemigrations` //staging
* Then run `python manage.py migrate` //applies the changes 

### App views.py and templates with models:
```python 
from .models import Hero
 
def index(request):
    context = {
    	"all_heroes": Hero.objects.all()
    }
    return render(request, "index.html", context)
```
```html
<h1>Heroes:</h1>
<ul>
{% for p in all_heroes %}
   <li>{{p.name}}</li>
{% endfor %}
</ul>

```
