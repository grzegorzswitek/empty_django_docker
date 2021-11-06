# Empty Django Dockerized app with Postgres, Gunicorn and Nginx (Dev + Prod)

1. Clone repository
* `git clone https://github.com/grzegorzswitek/empty_django_docker.git`
* `mv empty_django_docker/ <project_name>/`
* `cd <project_name>`
2. Run development container
* `sudo docker-compose -f docker-compose.dev.yml build web`
* `sudo docker-compose -f docker-compose.dev.yml run web django-admin startproject <project_name> .`
* `sudo docker-compose -f docker-compose.dev.yml up`
* open in browser `localhost:8000`
3. Modify Django settings
* open `app/<project_name>/settings.py`
* add:
`import os`
* change:
```
SECRET_KEY = os.environ.get("SECRET_KEY") # copy SECRET_KEY to .env.dev and .env.prod

DEBUG = int(os.environ.get("DEBUG", default=0))

ALLOWED_HOSTS = os.environ.get("DJANGO_ALLOWED_HOSTS").split(" ")

DATABASES = {
    "default": {
        "ENGINE": os.environ.get("SQL_ENGINE", "django.db.backends.sqlite3"),
        "NAME": os.environ.get("SQL_DATABASE", BASE_DIR / "db.sqlite3"),
        "USER": os.environ.get("SQL_USER", "user"),
        "PASSWORD": os.environ.get("SQL_PASSWORD", "password"),
        "HOST": os.environ.get("SQL_HOST", "localhost"),
        "PORT": os.environ.get("SQL_PORT", "5432"),
    }
}

STATIC_URL = '/static/'
STATIC_ROOT = BASE_DIR / "staticfiles"

MEDIA_URL = "/media/"
MEDIA_ROOT = BASE_DIR / "mediafiles"
```
5. Add superuser
in new terminal:
`sudo docker exec -it <project_name>_web_1 bash`  
`# python manage.py migrate`  
`# python manage.py createsuperuser`  
`# (...)`  
`# exit`  
open in browser `localhost:8000/admin` and log in  
6. Production
* `sudo docker-compose -f docker-compose.dev.yml down`
* change allowed host in .env.prod
* change <project_name> in ./nginx/nginx.conf
* change <project_name> in ./docker-compose.prod.yml
* `sudo docker-compose -f docker-compose.prod.yml up`  
in new terminal:  
`sudo docker exec -it <project_name>_web_1 bash`  
`# python manage.py migrate`  
`# python manage.py createsuperuser`  
`# (...)`  
`# python manage.py collectstatic`  
`# exit`  
open in browser localhost:1337/admin and log in  
