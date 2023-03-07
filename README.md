# Django
Some commands in Django

<U>Virtual Environment</U>
```
python -m venv venv 

(venv) C:\Users\Your Name>py -m pip install Django 

(venv) C:\Users\Your Name>django-admin --version 
```

**_New project called 'my_tennis_club':_**

```
django-admin startproject my_tennis_club 

py manage.py runserver 
```
**create a new Django application called 'members'**
```
py manage.py startapp members 
```
**Views **

view called 'members' that will return 'Hello World!'  

from django.shortcuts import render 
from django.http import HttpResponse 
 
def members(request): 
    return HttpResponse("Hello world!") 

 

send a variable named 'points' with the value 5 into the template 

from django.http import HttpResponse 
from django.template import loader 
 
def members(request): 
  template = loader.get_template('mytemplate.html') 
  context = { 
    'points': 5, 
  } 
  return HttpResponse(template.render(context, request)) 

 

from django.http import HttpResponse 
from django.template import loader 
from .models import Member 
 
def members(request): 
  mymembers = Member.objects.all().values() 
  template = loader.get_template('all_members.html') 
  context = { 
    'mymembers': mymembers, 
  } 
  return HttpResponse(template.render(context, request)) 

 

def details(request, id): 
  mymember = Member.objects.get(id=id) 
  template = loader.get_template('details.html') 
  context = { 
    'mymember': mymember, 
  } 
  return HttpResponse(template.render(context, request)) 

 

def main(request): 
  template = loader.get_template('main.html') 
  return HttpResponse(template.render()) 
 

**URLS** 

Create a file named urls.py in the same folder as the views.py file, and type this code in it: 

my_tennis_club/members/urls.py: 

from django.urls import path 
from . import views 
 
urlpatterns = [ 
    path('members/', views.members, name='members'), 
] 
 

list of paths of how to handle incomming requests 

my_tennis_club/my_tennis_club/urls.py: 

from django.contrib import admin 
from django.urls import include, path 
 
urlpatterns = [ 
    path('', include('members.urls')), 
    path('admin/', admin.site.urls), 
] 

 

my_tennis_club/members/urls.py: 

from django.urls import path 
from . import views 
 
urlpatterns = [ 

    path('', views.main, name='main'), 

    path('members/', views.members, name='members'), 

    path('members/details/<int:id>', views.details, name='details'), 
] 

 

**Syntax **

<h1>Hello {{ firstname }}, how are you?</h1> 

 

if statement: 

{% if greeting == 1 %} 
  <h1>Hello</h1> 
{% elif greeting == 2 %} 
  <h1>Welcome</h1> 
{% else %} 
  <h1>Goodbye</h1> 
{% endif %} 
 

Parentheses are not allowed in if statements in Django, so when you combine and and or operators, it is important to know that parentheses are added for and but not for or. 

{% if (greeting == 1 and day == "Friday") or greeting == 5 %} 
 

To check if a certain item is not present in an object. 

{% if 'Banana' not in fruits %} 
  <h1>Hello</h1> 
{% else %} 
  <h1>Goodbye</h1> 
{% endif %} 

 

is 

Check if two objects are the same. 

This operator is different from the == operator, because the == operator checks the values of two objects, but the is operator checks the identity of two objects. 

from django.http import HttpResponse 
from django.template import loader 
 
def testing(request): 
  template = loader.get_template('template.html') 
  context = { 
    'x': ['Apple', 'Banana', 'Cherry'],  
    'y': ['Apple', 'Banana', 'Cherry'],  
  } 
  return HttpResponse(template.render(context, request))   

 
**Different objects**

{% if x is y %} 
  <h1>YES</h1> 
{% else %} 
  <h1>NO</h1> 
{% endif %} 
NO 

 
**Different objects same values**

{% if x == y %} 
  <h1>YES</h1> 
{% else %} 
  <h1>NO</h1> 
{% endif %} 

YES 

 

**Same objects** 

{% with var1=x var2=x %} 
  {% if var1 is var2 %} 
    <h1>YES</h1> 
  {% else %} 
    <h1>NO</h1> 
  {% endif %} 
{% endwith %} 

 

**Comments **

<h1>Welcome Everyone</h1> 
{% comment "this was the original welcome message" %} 
    <h1>Welcome ladies and gentlemen</h1> 
{% endcomment %} 

<h1>Welcome{# Everyone#}</h1> 

**For Loops **

{% for x in cars %} 
  <h1>{{ x.brand }}</h1> 
  <p>{{ x.model }}</p> 
  <p>{{ x.year }}</p> 
{% endfor %} 

 

{% for x in members %} 
  <h1>{{ x.id }}</h1> 
  <p> 
    {{ x.firstname }} 
    {{ x.lastname }} 
  </p> 
{% endfor %} 

 

{% for x in fruits reversed %} 
  <p>{{ x }}</p> 
{% endfor %} 

 

The empty keyword can be used if you want to do something special if the object is empty. 

<ul> 
  {% for x in emptytestobject %} 
    <li>{{ x.firstname }}</li> 
  {% empty %} 
    <li>No members</li> 
  {% endfor %} 
</ul> 

 

The empty keyword can also be used if the object does not exist: 

<ul> 
  {% for x in myobject %} 
    <li>{{ x.firstname }}</li> 
  {% empty %} 
    <li>No members</li> 
  {% endfor %} 
</ul> 

 

create variables directly in the template 

{% with firstname="Tobias" %} 
<h1>Hello {{ firstname }}, how are you?</h1> 

 

Loop Variables 

Django has some variables that are available for you inside a loop: 

    forloop.counter 

    forloop.counter0 

    forloop.first 

    forloop.last 

    forloop.parentloop 

    forloop.revcounter 

    forloop.revcounter0 

 

<ul> 
  {% for x in fruits %} 
    <li>{{ forloop.counter }}</li> 
  {% endfor %} 
</ul> 

 

This is done in the settings.py file in the my_tennis_club folder. 

Look up the INSTALLED_APPS[] list and add the members app like this: 

INSTALLED_APPS = [ 
    'django.contrib.admin', 
    'django.contrib.auth', 
    'django.contrib.contenttypes', 
    'django.contrib.sessions', 
    'django.contrib.messages', 
    'django.contrib.staticfiles', 
    'members' 
] 

py manage.py migrate 

py manage.py runserver 

 

Include 

<h1>Hello</h1> 
 
<p>This is a Django template with a footer.</p> 
 
{% include 'footer.html' %} 

 

templates/mymenu.html: 

<div>HOME | {{ me }} | ABOUT | FORUM | {{ sponsor }}</div> 

 
templates/template.html: 

<!DOCTYPE html> 
<html> 
<body> 
 
{% include "mymenu.html" with me="TOBIAS" sponsor="W3SCHOOLS" %} 
 
<h1>Welcome</h1> 
 
<p>This is my webpage</p> 
 
</body> 
</html> 

 

 

Template 

Create a templates folder inside the members folder, and create a HTML file named myfirst.html. 

<!DOCTYPE html> 
<html> 
<body> 
 
<h1>Hello World!</h1> 
<p>Welcome to my first Django project!</p> 
 
</body> 
</html> 

 

my_tennis_club/mystaticfiles/mystyles.css: 

@import url('https://fonts.googleapis.com/css2?family=Source+Sans+Pro:wght@400;600&display=swap'); 
body { 
  margin:0; 
  font: 600 18px 'Source Sans Pro', sans-serif; 
  letter-spacing: 0.64px; 
  color: #585d74; 
} 
.topnav { 
  background-color:#375BDC; 
  color:#ffffff; 
  padding:10px; 
} 
.topnav a:link, .topnav a:visited { 
  text-decoration: none; 
  color: #ffffff;  
} 
.topnav a:hover, .topnav a:active { 
  text-decoration: underline; 
} 
.mycard { 
  background-color: #f1f1f1; 
  background-image: linear-gradient(to bottom, #375BDC, #4D70EF);  
  background-size: 100% 120px; 
  background-repeat: no-repeat; 
  margin: 40px auto; 
  width: 350px; 
  border-radius: 5px; 
  box-shadow: 0 5px 7px -1px rgba(51, 51, 51, 0.23);  
  padding: 20px; 
} 
 

 

my_tennis_club/members/templates/master.html: 

{% load static %} 
<!DOCTYPE html> 
<html> 
<head> 
  <link rel="stylesheet" href="{% static 'mystyles.css' %}"> 
  <title>{% block title %}{% endblock %}</title> 
</head> 
<body> 
 
<div class="topnav"> 
  <a href="/">HOME</a> | 
  <a href="/members">MEMBERS</a> 
</div> 
 
{% block content %} 
{% endblock %} 
 
</body> 
</html> 

 

my_tennis_club/members/templates/main.html: 

{% extends "master.html" %} 
 
{% block title %} 
  My Tennis Club 
{% endblock %} 
 
 
{% block content %} 
  <h1>My Tennis Club</h1> 
 
  <h3>Members</h3> 
   
  <p>Check out all our <a href="members/">members</a></p> 
   
{% endblock %} 

 

The list in all_members.html should be clickable, and take you to the details page with the ID of the member you clicked on: 

my_tennis_club/members/templates/all_members.html 

{% extends "master.html" %} 
 
{% block title %} 
  My Tennis Club - List of all members 
{% endblock %} 
 
 
{% block content %} 
  <div class="mycard"> 
    <h1>Members</h1> 
    <ul> 
      {% for x in mymembers %} 
        <li onclick="window.location = 'details/{{ x.id }}'">{{ x.firstname }} {{ x.lastname }}</li> 
      {% endfor %} 
    </ul> 
  </div> 
{% endblock %} 
 

my_tennis_club/members/templates/details.html 

{% extends "master.html" %} 
 
{% block title %} 
  Details about {{ mymember.firstname }} {{ mymember.lastname }} 
{% endblock %} 
 
{% block content %} 
  <div class="mycard"> 
    <h1>{{ mymember.firstname }} {{ mymember.lastname }}</h1> 
    <p>Phone {{ mymember.phone }}</p> 
    <p>Member since: {{ mymember.joined_date }}</p> 
  </div> 
{% endblock %} 
 

 
 

Set the debug property to False, and allow the project to run from your local host: my_tennis_club/my_tennis_club/settings.py: 

. 
. 
# SECURITY WARNING: don't run with debug turned on in production! 
DEBUG = False 
 
ALLOWED_HOSTS = ['*'] 
. 
. 

Important: When DEBUG = False, Django requires you to specify the hosts you will allow this Django project to run from. 

In production, this should be replaced with a proper domain name: 

ALLOWED_HOSTS = ['yourdomain.com'] 

 

my_tennis_club/members/templates/404.html: 

<!DOCTYPE html> 
<html> 
<title>Wrong address</title> 
<body> 
 
<h1>Ooops!</h1> 
 
<h2>I cannot find the file you requested!</h2> 
 
</body> 
</html> 

 

Models 

Django models are specified as classes in the models.py file. 

my_tennis_club/members/models.py: 

from django.db import models 
 
class Member(models.Model): 
  firstname = models.CharField(max_length=255) 
  lastname = models.CharField(max_length=255) 
  phone = models.IntegerField(null=True) 
  joined_date = models.DateField(null=True) 
 
  def __str__(self): 
    return f"{self.firstname} {self.lastname}" 

 

changes in the models.py file that affects the database, you must run two migration commands to make the model synchronized with the database. 

py manage.py makemigrations members 
 

Django creates a file describing the changes and stores the file in the /migrations/  

folder: my_tennis_club/members/migrations/0001_initial.py 

 

 
The table is not created yet, you will have to run one more command, then Django will create and execute an SQL statement, based on the content of the new file in the /migrations/ folder.  

py manage.py migrate 

 

a field called 'points', that must be a integer of some sort, positive or negative 

from django.db import models 
 
class Test(models.Model): 
  points = models.IntegerField() 

 
Admin 

py manage.py createsuperuser 

To include the Member model in the admin interface, we have to tell Django that this model should be visible in the admin interface. 

 

my_tennis_club/members/admin.py: 

from django.contrib import admin 
 
from .models import Member 
 
# Register your models here. 

class MemberAdmin(admin.ModelAdmin): 
  list_display = ("firstname", "lastname", "joined_date",) 

 

To make your models viewable in the admin interface, they have to be registered in the admin.py file. 

admin.site.register(Member) 

 

Queryset 

A QuerySet is a collection of data from a database. 

A QuerySet is built up as a list of objects. 

QuerySets makes it easier to get the data you actually need, by allowing you to filter and order the data at an early stage. 

 

The values() Method 

from django.http import HttpResponse 
from django.template import loader 
from .models import Member 
 
def testing(request): 
  mydata = Member.objects.all().values() 

  # Return Specific Columns  # mydata = Member.objects.values_list('firstname') 

  # Return Specific Rows  # mydata = Member.objects.filter(firstname='Emil').values() 

   

  # Return records where lastname is "Refsnes" and id is 2:   

  # mydata = Member.objects.filter(lastname='Refsnes', id=2).values() 

 

  # .filter(firstname__startswith='L'); 
 

 
  template = loader.get_template('template.html') 
  context = { 
    'mymembers': mydata, 
  } 
  return HttpResponse(template.render(context, request)) 

 

Return records where firstname is either "Emil" or Tobias": 

mydata = Member.objects.filter(firstname='Emil').values() | Member.objects.filter(firstname='Tobias').values() 

 

from django.http import HttpResponse 
from django.template import loader 
from .models import Member 
from django.db.models import Q 
 
def testing(request): 
  mydata = Member.objects.filter(Q(firstname='Emil') | Q(firstname='Tobias')).values() 
  template = loader.get_template('template.html') 
  context = { 
    'mymembers': mydata, 
  } 
  return HttpResponse(template.render(context, request)) 

 

 

records where firstname starts with the capital letter 'L' 

mydata = Member.objects.filter(firstname__startswith='L').values() 

 

case-insensitive search to return the records where 'firstname' starts with the letter 'L' 

Member.objects.filter(firstname__istartswith='L').values() 

 

sort the result aplhabetically on firstname 

Member.objects.all().order_by('firstname').values() 

 

sort the result aplhabetically DESCENDING on the field 'firstname' 

Member.objects.all().order_by('-firstname').values() 

 

Order the result first by lastname ascending, then descending on id: 

mydata = Member.objects.all().order_by('lastname', '-id').values() 

 

 

Static Files 

settings.py 

# SECURITY WARNING: don't run with debug turned on in production! 

DEBUG = False 
ALLOWED_HOSTS = [] 

 

# SECURITY WARNING: don't run with debug turned on in production! 
DEBUG = False 

ALLOWED_HOSTS = ['*'] 

 

When collecting static files for your project, you have to specify where to collect them 

STATIC_ROOT = BASE_DIR / 'productionfiles' 
 
STATIC_URL = 'static/' 

 

When collecting static static files for your project, you have to run a specific command 

py manage.py collectstatic  

 

{% load static %} 

<!DOCTYPE html> 
<html> 
<link rel="stylesheet" href="{% static 'styles.css' %}"> 
<body> 
 
<h1>Hello</h1> 
 
</body> 
</html> 

 

WhiteNoise 

Django does not have a built-in solution for serving static files, at least not in production when DEBUG has to be False. 

We have to use a third-party solution to accomplish this. 

pip install whitenoise 

my_tennis_club/my_tennis_club/settings.py: 

MIDDLEWARE = [ 
    'django.middleware.security.SecurityMiddleware', 
    'django.contrib.sessions.middleware.SessionMiddleware', 
    'django.middleware.common.CommonMiddleware', 
    'django.middleware.csrf.CsrfViewMiddleware', 
    'django.contrib.auth.middleware.AuthenticationMiddleware', 
    'django.contrib.messages.middleware.MessageMiddleware', 
    'django.middleware.clickjacking.XFrameOptionsMiddleware', 
    'whitenoise.middleware.WhiteNoiseMiddleware', 
] 

 

my_tennis_club/my_tennis_club/settings.py: 

In the STATICFILES_DIRS list, you can list all the directories where Django should look for static files. 

The BASE_DIR keyword represents the root directory of the project, and together with the / "mystaticfiles", it means the mystaticfiles folder in the root directory. 

 

STATIC_ROOT = BASE_DIR / 'productionfiles' 
STATIC_URL = 'static/' 

STATICFILES_DIRS = [ 
    BASE_DIR / 'mystaticfiles' 
] 

Every time you make a change in a static file, you must run the collectstatic command to make the changes take effect: 

py manage.py collectstatic 

py manage.py runserver 
