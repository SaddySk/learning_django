# learning_django

@ use this token when using form
	` {% csrf_token %}    <!-- to prevent CSRF attack (settings.py -> crsf-middleware)-->


````````````````````````````````````````````````````````````````````````````````````````````````````````````````
@ NOTATIONS w.r.t. the "firstProject"


+ Create Project
	- django-admin startprjoect <projectName - firstProject>

+ Run Your Application
	- python3 manage.py runserver



FIRST APP TUTORIAL
==================

+ Create an App
	- python3 manage.py startapp <appName - calc>
+ Create [urls.py] file insdie <calc>
+ Edit [view.py] file in <calc>					->	Setup 'home(request)' [frontend]
+ include <calc.urls> file to <firstProject> file		Note: from django.urls import include



# CREATING A VIEW (DTL)
-----------------------
xxxxxxx	WORK FLOW	xxx	=>	urls.py(set path) > views.py(send data to print) > home.html(print data)
xxxxxxxxxxxxxxxxxxxxxxxxxxx

+ create a TEMPLATE folder in root directory of the project
+ crete HTML files inside TEMPLATE as per your requirement

+ add TEMPLATE to "settings.py" inside <firstProject>
	- 'DIRS' : [os.path.join(BASE_DIR, '<templateFolderName - templates>')]
+ go to 'views.py' in <calc>
	- return render(request, '<htmlPage.html - home.html>')
	- To send dynamic values UPDATE [views.py>] & [home.html] file
		=	return render(request, 'home.html', {'name': '<data>'})	->	"views.py"
		=	use -> {{name}}							->	"home.html"

@ Connecting to ".html" files
	+ create an html file and have a block with SYNTAX	-	{% block content %}	...	{% endblock %}
	+ extend above '.html' file to another '.html' file wher you to send connect
		SYNTAX  - {% extend <fileName.html - base.html> %}	{% block content%}	...	{% endblock %}




# Addition of Two Numbers
-------------------------
xxxxxxx	WORKF FLOW	xxx	=>	urls.py(set path) > home.html(get data form web) > views.py(operation in add()) > result.html(print data)
xxxxxxxxxxxxxxxxxxxxxxxxxxx

+ create a 'form' in [home.html]
+ create [result.html] 
+ add 'path' to [urls.py]	->	! path('add', views.add, name='add') !
+ create 'add()' function in [views.py]



# Using frontend templates
--------------------------

+ place 'index.html' IN templates
+ setup 'urls.py' & 'views.py' for all as done before
+ create folder 'design'
+ place * downloaded folders in 'design'

+ CHANGES IN 'settings.py'
	` STATICFILES_DIRS = [os.path.join(BASE_DIR, '<design>')]
	  STATIC_ROOT = os.path.join(BASE_DIR, 'assests')
	
	` run the following command
		>> python3 manage.py collectstatic

+ CHANGES IN 'templates/index.html'
	` add this in first line
		- {% load static %}

	` update * the links
		- FROM > href="#"
		- TO   > href="{% static '#' %}"



# Dynamic value 1
-----------------
+ create objects in 'models.py'
+ import and initialize object into 'views.py' and pass it to 'index.html'
+ update 'index.html' inorder to use dynamic values from "object"


# Dynamic value 2 (passing multiple unknown values)
--------------------------------------------------
+ pass multiple objects in the form of lists
+ use for loop in 'index.html' to automate the process with respect to dynamic values


@@ FOR LOOP
	` {% for _ in xyz %}	...	{% endfor %}

@@ IF STATEMENT
	` {% if <condition> %}	...	{% endif %}


# Dynamic value 3 (passing images)
----------------------------------
+ since we can't use jinja format (to use imported image object) inside jinja format (used as default)
+ add the "statement" in the first line
	` {% static "images" as baseUrl %}
+ update html to use 'image'
	` src="{{baseUrl}}/{{img}}"



# Connect with database
-----------------------
+ Changes in 'settings.py' under 'firstproject'
	`	'ENGINE': 'django.db.backends.postgresql',
        'NAME': '<tablename - firstproject>',
        'USER': '<postgres - default by postgresql>',
        'PASSWORD': 'onlyforme',
        'HOST': 'localhost'

+ connect with database	(Requirement - psycopg2 {connector})
+ create a model for the database schema in 'models.py'

@MIGRATION
+ to create tables in DB - migrate models
	` 1st -> specify the app in installed apps in 'settings.py' under "firstproject"
		>>> 'usingtemplate.apps.UsingtemplateConfig'
	` Next -> run the following command to create migration file
		>>> python manage.py makemigrations
	` Now -> create schema to be migrated to the DB
		>>> python manage.py sqlmigrate <appname - usingtemplate> <migrationfilename - 0001>
	` Finally -> do the migration
		>>> python manage.py migrate

@RE-MIGRATION (if you make any changes in the models and want its effect in the DB)

>>> python manage.py makemigrations
		(select preferable option from the suggestions)
>>>	python manage.py migrate



# Working with ADMIN PANEL
--------------------------
+ first create an "USER" if does not exist
	>>> python manage.py createsuperuser

+ to get the UI page to server data to the DB "register models" in the "admin.py" file
	` from .models import <classname - Plan>
	` admin.site.register(<classname - Plan>)



# User and DB interaction
-------------------------
* to upload any media files make below changes
+ add "path of dir" and "URL" to 'settings.py' where you want to save your media files
	` MEDIA_URL  = '/<path -media>/'
	  MEDIA_ROOT = os.path.join(BASE_DIR, '<dir - media>')

+ add "URLS" in 'firstproject/urls.py'
	` from django.conf import settings
	  from django.confo.urls.static imprt static
	>>>	urlpatterns += static(settings.MEDIA_URL, document_root = settings.MEDIA_ROOT)

+ update "image statement" in 'index.html'
	` FROM	"{{baseUrl}}/{{plan.img}}"	TO	"{{plan.img.url}}"

+ update 'views.py'
	` plans = Plan.objects.all()



# User Registration 1
---------------------
+ create a module/app 'accounts'
	` python manage.py startapp accounts
+ create 'urls.py'
+ add path to 'firstproject/urls.py'
+ create view at 'views.py'
+ add "register" button in 'index.html'
+ create 'register.html' in 'templates'


# User Registration 2 (GET/POST)
--------------------------------
+ sending the data to DB
	` from django.contrib.auth.models import User, auth
	` username = request.POST['<i/p field name in html form - username>']
	` password1 = request.POST['<i/p fiels name in html form - password1>']
	` user = User.objects.create_user(<DB col. name - username>="username", <DB col. name - password>="password1")
	` user.save()
	` return redirect('/')								OPTIONAL

+ to redirect to another page after the work is done
	` from django.shortcuts import redirect
	` return redirect('<path - '/'>')

# User Registration 2 (CRED VERIFICATION)
-----------------------------------------
	` User.objects.filter(username=username).exists()

# User Registration 2 (PASSING MESSAGES)
----------------------------------------
	` from django.contrib import messages
	` messages.info(request, 'message to be shown')
	`` update 'register.html' to print messages in webpage
		` {% for message in messages %}
            {{message}}
          {% endfor %}



# Login
-------
+ set path in 'urls.py'
+ create 'login.html'
+ update 'views.py'
	` from django.contrib.auth.models import User, auth
	` read 'username' and 'password'
	` user = auth.authenticate(username=usermame, password=password)			# authentication
	` if user is not None:
            auth.login(request, user)											# login


# Logout
--------
+ set path in 'urls.py'
+ update 'views.py'
	` auth.logout(request)


@ Dynamic pagic seperately for LOGIN & LOGOUT

    {% if user.is_authenticated %}

    <li>Hello, {{user.first_name}}</li>
    <li><a href="accounts/logout" class="nav-link">Logout</a></li>

    {% else %}

    <li><a href="accounts/register" class="nav-link">Register</a></li>
    <li><a href="accounts/login" class="nav-link">Login</a></li>

    {% endif %}



CREDIT - Navin Reddy AKA TELUSKO
