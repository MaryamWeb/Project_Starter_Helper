# Django

### Create a virtual environment:
* In terminal type `py -m venv ENV_NAME`
* Activate the virtual environment `call ENV_NAME\Scripts\activate`
* With your virtual environment activated In the terminal run `pip install Django`   
* Make sure we have successfully installed Django by checking its version. In terminal run `Django --version`

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
* Create an empty folder `mkdir templates`  
* Inside the folder create an html template `index.html`

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

 