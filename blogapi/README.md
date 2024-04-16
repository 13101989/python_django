# Steps to build

```bash
cd <root>/blogapi
python -m venv .venv 
source .venv/Scripts/activate
python -m pip install django~=4.0.0
python -m pip install djangorestframework~=3.13.0

django-admin startproject django_project .

python manage.py startapp accounts
python manage.py makemigrations accounts
python manage.py migrate

python manage.py startapp posts
python manage.py makemigrations posts
python manage.py migrate
python manage.py test posts

python manage.py createsuperuser

python manage.py runserver
```

# Prepare Heroku deployment

```bash
mkdir static
cd static
python -m pip install whitenoise==5.3.0 # tool that helps to serve static files
python manage.py collectstatic
python -m pip install gunicorn~=20.1.0 # will be used as production web server for the application
python -m pip freeze > requirements.txt
mkdir runtime.txt # insert python version to be used
mkdir Procfile # insert start command for the app
```

# Deploy to Heroku

```bash
heroku login
heroku create python-django-blogapi
cd <root>
git push heroku `git subtree split --prefix blogapi main`:main --force # push only blogapi folder from repo, overwrites existing main branch from heroku
heroku ps:scale web=1 # deploy on one container 
heroku open # open app url
heroku ps:scale web=0 # remove deployment
heroku logs --tail # check logs for deployment
```