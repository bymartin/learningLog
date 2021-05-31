# learningLog

Learning Log
an online journal system that lets you keep track of
information you’ve learned about particular topics

From the book Python Crash Course, 2nd edition

Django (https://djangoproject.com/)

## Getting your environment setup

>cd learningLog
>python3 -m venv ll_env
>source ll_env/bin/activate

To stop using the virtual environment, 'deactivate'

## Install Django
>pip install django

## Creating a project in Django
(ll_env)learning_log$ django-admin startproject learning_log .

(ll_env)learning_log$ ls
learning_log ll_env manage.py

(ll_env)learning_log$ ls learning_log
__init__.py settings.py urls.py wsgi.py

### Creating a database
>python manage.py migrate
>ls
You will see db.sqlite3 created (SQLite database)

## Viewing the project
>python manage.py runserver

If you receive the error message That port is already in use, tell Django to use
a different port by entering python manage.py runserver 8001, and then cycle
through higher numbers until you find an open port.

## Starting an app

Leave the development server running. Open a new tab and activate the
virtual environment.

>cd /dev/python/learningLog
>source ll_env/bin/activate

>python manage.py startapp learning_logs

## Defining models
models.py in the app directory

Django Model Field Reference at
https://docs.djangoproject.com/en/2.2/ref/models/fields/

## Activating Models
open settings.py in learning_log directory
add our new app under INSTALLED_APPS

Tell django to modify the db for it to store the Topic info.
> python manage.py makemigrations learning_logs
Migrations for 'learning_logs':
  learning_logs/migrations/0001_initial.py
    - Create model Topic
>python manage.py migrate

Whenever you modify the data that Learning Log manages, you need
to do three steps.
1. modify models.py
2. call makemigrations on learning_logs
3. tell Django to migrate the project

## Django Admin Site

Create a superuser:
python manage.py createsuperuser
user: ll_admin

Registering a model with the admin site:
Modify admin.py in the app directory

Login into the admin site with superuser account:
http://localhost:8000/admin/

### Defining the entry model
Modify models.py to specify what kind of entries can be made
for a certain topic.
After to modify the model, do the standard next steps:
>python manage.py makemigrations learning_logs
>python manage.py migrate

### Register entry with the admin site
admin.py

### Django shell
>python manage.py shell

### Making pages - Home page
define URL, write view, write template

base URL, http://localhost:8000/
Change this to map the base URL to the home page.

Modify default urls.py in learning_log folder.
Make a new urls.py in learning_logs folder.

Writing a view:
modify view.py in learning_logs folder

Write template:
In learning_logs folder, make a new folder called templates.
Inside that, make a new folder called learning_logs.
Make a new file called index.html.

### Template Inheritance

Parent template:
Create base.html in same directory as index.html

Rewrite index.html to inherit from base.html (child template)

Topics page:
modify urls.py
modify views.py
make topics.html

### User Forms

Topic ModelForm
Create forms.py in the same folder as models.py
new_topic URL for creating a new topic
create new_topic view function and template

Now we can create new topics, now need to be able to add entries
Add EntryForm class to forms.py
new_entry URL, new_entry view function and template

Editing Entries:
edit_entry URL, edit_entry view and template

## User Accounts

Create new app called users
> python manage.py startapp users
Add new app to settings.py
Modify root urls.py
Make a urls.py in the users folder

Inside the users directory,
> mkdir -p templates/registration
Make a login.html here

Connect data to certain users
- add a foreign key relationship to a user in models.py
Then migrate DB
python manage.py makemigrations learning_logs
Pick 1 and then id of one of the users
python manage.py migrate

Restricting topics access to correcd users:
User should only see the topics they own
Change views.py

Protecting a user's topic:
Any registered user could find a topic by accident - 
    http://localhost:8000/topics/3/ for example and might get a match
Modify views.py to add a check

## Making the app look better and deployment

Styling - use Bootstrap library
Deploy using Heroku

### django-bootstrap4 App

> pip install django-bootstrap4

https://getbootstrap.com/
Click examples to see templates

We will use this one:
https://getbootstrap.com/docs/5.0/examples/navbar-static/

## Deployment using Heroku
https://www.heroku.com/
https://devcenter.heroku.com/categories/python-support

Install the Heroku CLI
https://devcenter.heroku.com/articles/heroku-cli

Install three packages that helps server Django projects
on a live server from your virtual environmnent:
pip install psycopg2-binary
pip install django-on-heroku
pip install gunicorn

Create a requirements.txt file
venv> pip freeze > requirements.txt
Heroku will install all the same packages listed in requirements.txt.
So it should behave the same as our local system.

Specifying the Python runtime:
venv> python --version
Python 3.8.5

Make a new file 'runtime.txt' in the same folder as manage.py.
Put the following:
python-3.8.5

Modify settings.py for Heroku.
Add following:.
import django_on_heroku
django_on_heroku.settings(locals())

Make a Procfile to start processes
This goes in the same folder as manage.py
It has one line:
web: gunicorn learning_log.wsgi --log-file -

Pushing to Heroku:
heroku login
heroku create
git push heroku master
git push heroku main

Project is now deployed. See public URL created.
You can also "heroku open" to open the web site.

Check server process started okay:
> heroku ps

Server is deployed but not ready. We need to setup the public database.
> heroku run python manage.py migrate
Now it may be used!

### Refine the deployment

Create a superuser on Heroku:
> heroku run bash
~ $ ls
learning_log  learning_logs  manage.py  Procfile  README.md  requirements.txt  runtime.txt  staticfiles  users
~ $ python manage.py createsuperuser

Add /admin/ to the end of the URL for the live app to login to the admin site.

Make the URL more user friendly:
heroku apps:rename new-name

Securing the live project:
We don't want the detailed debug we pages shown on the live deployment.
Change settigns.py
# Heroku settings
import os
import django_on_heroku
django_on_heroku.settings(locals())

if os.environ.get('DEBUG') == 'TRUE':
    DEBUG = True
elif os.environ.get('DEBUG') == 'FALSE':
    DEBUG = Fals

Commit and push changes


























