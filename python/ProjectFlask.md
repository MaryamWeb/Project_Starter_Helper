# Flask 

### Create a virtual environment:
* In terminal type `py -m venv ENV_NAME`
* Activate the virtual environment `call ENV_NAME\Scripts\activate`
* With your virtual environment activated In the terminal run `pip install Flask`   
* Make sure we have successfully installed Flask by checking its version. In terminal run `flask --version`

### Setup
* Create an empty folder `mkdir greet_flask` -->  `cd greet_flask`
* Inside the folder, create a file called app.py `touch app.py`

### app.py
```python
from flask import Flask  
app = Flask(__name__)   
@app.route('/')           
def greet():
    return 'Hello All!'  
     
if __name__=="__main__":   # Ensure this file is being run directly and not from a different module    
    app.run(debug=True)    # Run the app in debug mode.
```
* Run the application by running the following command `python app.py`

Check our server at localhost:5000/

### Routes:
Add another route to our app.py file:
```python
@app.route('/hello/<name>')  
def hello(name):
    print(name)
    return "Hello, " + name
```
### Rendering Views:
* create the directory and file `/hello_flask/templates/index.html`
* Refer to our HTML files like in `/hello_flask/hello.py`
```python
from flask import Flask, render_template  
app = Flask(__name__)                     
    
@app.route('/')                           
def greet():
    return render_template('index.html')  
    
if __name__=="__main__":
    app.run(debug=True)  
```
### Template Engines:(Passing Data to the HTML)
```python
from flask import Flask, render_template
app = Flask(__name__)

@app.route('/')
def index():
    return render_template("index.html", phrase="Hello", times=5)	 

if __name__=="__main__":
    app.run(debug=True)
```
### Rendering Data on a Template
There are 2 special inputs we can use to insert Python-like code into our Flask templates.
* {{ First }}
* {% Second %}

```html
<h3>My Flask Template</h3>
<p>Phrase: {{ phrase }}</p>
<p>Times: {{ times }}</p>
      
{% for x in range(0,times): %}
    <p>{{ phrase }}</p>
{% endfor %}
      
{% if phrase == "hello" %}
  <p>The phrase says hello</p>
{% endif %}
```

### Static Files:
* create a directory called static
* in static create folders css, img, js
* We can then access them in our HTML templates, like so:
 ```html
<link rel="stylesheet" type="text/css" href="{{ url_for('static', filename='css/my_style.css') }}">
<script type="text/javascript" src="{{ url_for('static', filename='js/my_script.js') }}"></script>
<img src="{{ url_for('static', filename='img/my_img.png') }}">
```

## Notes:
To insert comments: {# ... #}


