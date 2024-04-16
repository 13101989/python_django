# Steps to build

```bash
cd <root>/library

django-admin startproject django_project .
python manage.py migrate
python manage.py createsuperuser

python manage.py startapp books
python manage.py makemigrations books
python manage.py migrate
python manage.py test books

python manage.py startapp apis
python manage.py test apis

python manage.py runserver
```

# Prepare Heroku deployment

```bash
python -m pip install whitenoise==6.0.0 # tool that helps to serve static files
python manage.py collectstatic
python -m pip install gunicorn~=20.1.0 # will be used as web server for the application
python -m pip freeze > requirements.txt
mkdir runtime.txt # insert python version to be used
mkdir Procfile # insert start command for the app
```

# Deploy to Heroku

```bash
heroku login
heroku create python-django-library
cd <root>
git subtree push --prefix library heroku main # push only library folder from repo
heroku ps:scale web=1 # deploy on one container 
heroku open # open app url
heroku ps:scale web=0 # remove deployment
heroku logs --tail # check logs for deployment
```