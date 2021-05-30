# learningLog

Learning Log
an online journal system that lets you keep track of
information you’ve learned about particular topics

From the book Python Crash Course, 2nd edition

Django (https://djangoproject.com/)

### Getting your environment setup

>cd learningLog
>python3 -m venv ll_env
>source ll_env/bin/activate

To stop using the virtual environment, 'deactivate'

### Install Django
>pip install django

### Creating a project in Django
(ll_env)learning_log$ django-admin startproject learning_log .

(ll_env)learning_log$ ls
learning_log ll_env manage.py

(ll_env)learning_log$ ls learning_log
__init__.py settings.py urls.py wsgi.py

### Creating a database
>python manage.py migrate
>ls
You will see db.sqlite3 created (SQLite database)

### Viewing the project
>python manage.py runserver

If you receive the error message That port is already in use, tell Django to use
a different port by entering python manage.py runserver 8001, and then cycle
through higher numbers until you find an open port.

### Starting an app

Leave the development server running. Open a new tab and activate the
virtual environment.

>cd /dev/python/learningLog
>source ll_env/bin/activate

>python manage.py startapp learning_logs

### Defining models
models.py in the app directory

Django Model Field Reference at
https://docs.djangoproject.com/en/2.2/ref/models/fields/

### Activating Models
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

### Django Admin Site

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

## Making pages - Home page
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

## Template Inheritance

Parent template:
Create base.html in same directory as index.html

Rewrite index.html to inherit from base.html (child template)

Topics page:
modify urls.py
modify views.py
make topics.html

## User Accounts

Topic ModelForm
Create forms.py in the same folder as models.py
new_topic URL for creating a new topic
create new_topic view function and template

Now we can create new topics, now need to be able to add entries
Add EntryForm class to forms.py
new_entry URL, new_entry view function and template

Editing Entries:
edit_entry URL, edit_entry view and template















