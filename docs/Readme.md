# folder structure
```
[ Sunday 26 February 2023 01:19:14 PM IST ]  [ simha@gauranga:/home/extra/simha/django_reactjs_project ]
 $ tree   
.
├── docker_setup_with_code_github_repo
│   ├── CODE_FOLDERS
│   │   ├── python_django_rest_api
│   │   └── reactjs_dashboard
│   ├── docker-compose-localhost-localdb.yml
│   ├── Docker_images
│   │   └── PYTHON_3_10_10_CUSTOM
│   │       └── Dockerfile
│   └── docs
│       └── Readme.md
└── DO_NOT_DELETE_postgres_data
```


# git init iniside the docker_setup_with_code_github_repo

```
$ git init 
```

# create python docker image

```
cd /home/extra/simha/django_reactjs_project/docker_setup_with_code_github_repo
cd $(PWD)/Docker_images/PYTHON_3_10_10_CUSTOM/Dockerfile
docker build -t python3.10.10_custom_build:latest --file Dockerfile .
```

# create django project (inside the docker container)

```
cd /home/extra/simha/django_reactjs_project/docker_setup_with_code_github_repo
hostfolder="$(pwd)/CODE_FOLDERS/python_django_rest_api"
dockerfolder="/home/simha/app"
docker run --rm -it \
  -v ${hostfolder}:${dockerfolder} \
python3.10.10_custom_build:latest /bin/bash

-- check the files

simha@9a9ace0cd6c4:~/app$ ls -al
total 12
drwxr-xr-x 3 simha simha 4096 Feb 26 05:53 .
drwxr-xr-x 1 simha simha 4096 Feb 26 05:32 ..

-- create virtualenv

simha@9a9ace0cd6c4:~/app$ pipenv shell
Creating a virtualenv for this project...
Pipfile: /home/simha/app/Pipfile
Using /usr/local/bin/python (3.10.10) to create virtualenv...
⠼ Creating virtual environment...created virtual environment CPython3.10.10.final.0-64 in 269ms
  creator CPython3Posix(dest=/home/simha/app/.venv, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/home/simha/.local/share/virtualenv)
    added seed packages: pip==23.0, setuptools==67.1.0, wheel==0.38.4
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator

✔ Successfully created virtual environment!
Virtualenv location: /home/simha/app/.venv
Creating a Pipfile for this project...
Launching subshell in virtual environment...
simha@9a9ace0cd6c4:~/app$  . /home/simha/app/.venv/bin/activate

-- check the files

(app) simha@9a9ace0cd6c4:~/app$ ls -al
total 20
drwxr-xr-x 4 simha simha 4096 Feb 26 05:54 .
drwxr-xr-x 1 simha simha 4096 Feb 26 05:54 ..
drwxr-xr-x 4 simha simha 4096 Feb 26 05:54 .venv
-rw-r--r-- 1 simha simha  139 Feb 26 05:54 Pipfile


-- install django 4.1

(app) simha@9a9ace0cd6c4:~/app$ pipenv install django=="4.1"
Installing django==4.1...
Resolving django==4.1...
Installing...
Adding django to Pipfile's [packages] ...
✔ Installation Succeeded
Pipfile.lock not found, creating...
Locking [packages] dependencies...
Building requirements...
Resolving dependencies...
✔ Success!
Locking [dev-packages] dependencies...
Updated Pipfile.lock (f6d1bf8c316f37f3475023af8b4b0f9883dfc61a99305165a1475b08e2ee370f)!
Installing dependencies from Pipfile.lock (ee370f)...

-- check installation

(app) simha@0cc1dc3a7606:~/app$ python3 -m django --version
4.1

-- Pipfile

(app) simha@0cc1dc3a7606:~/app$ cat Pipfile
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
django = "==4.1"

[dev-packages]

[requires]
python_version = "3.10"

```

# create django project (inside the docker container)

```
-- list files

(app) simha@0cc1dc3a7606:~/app$ ls -al
total 20
drwxr-xr-x 3 simha simha 4096 Feb 26 07:53 .
drwxr-xr-x 1 simha simha 4096 Feb 26 07:52 ..
drwxr-xr-x 5 simha simha 4096 Feb 26 07:52 .venv
-rw-r--r-- 1 simha simha  156 Feb 26 07:53 Pipfile
-rw-r--r-- 1 simha simha 1440 Feb 26 07:53 Pipfile.lock


-- create project  (note dot at end the end)

(app) simha@0cc1dc3a7606:~/app$ django-admin startproject petsproject .

-- list files (we see manage.py and petsproject folder created)

(app) simha@0cc1dc3a7606:~/app$ ls -al
total 28
drwxr-xr-x 4 simha simha 4096 Feb 26 07:55 .
drwxr-xr-x 1 simha simha 4096 Feb 26 07:52 ..
drwxr-xr-x 5 simha simha 4096 Feb 26 07:52 .venv
-rw-r--r-- 1 simha simha  156 Feb 26 07:53 Pipfile
-rw-r--r-- 1 simha simha 1440 Feb 26 07:53 Pipfile.lock
-rwxr-xr-x 1 simha simha  667 Feb 26 07:55 manage.py
drwxr-xr-x 2 simha simha 4096 Feb 26 07:55 petsproject

-- tree view

(app) simha@0cc1dc3a7606:~/app$ tree
.
├── Pipfile
├── Pipfile.lock
├── manage.py
└── petsproject
    ├── __init__.py
    ├── asgi.py
    ├── settings.py
    ├── urls.py
    └── wsgi.py

1 directory, 8 files

```

# SETTING UP POSTGRESQL DATABASE

# create DO_NOT_DELETE_postgres_data

```
[ Sunday 26 February 2023 01:28:08 PM IST ]  [ simha@gauranga:/home/extra/simha/django_reactjs_project ]
 $ ls -al
total 16
drwxr-xr-x 4 simha santhosh 4096 Feb 26 13:18 .
drwx------ 4 simha santhosh 4096 Feb 26 09:43 ..
drwxr-xr-x 6 simha santhosh 4096 Feb 26 12:35 docker_setup_with_code_github_repo
drwxr-xr-x 2 simha santhosh 4096 Feb 26 12:02 DO_NOT_DELETE_postgres_data
```

# install postgresql server

-- installed as a seperate container from docker-compose.yml
-- dbame: gauranga, user: simha, pass: krishna

```
  postgresql:
    image: "postgres:13-alpine"
    restart: always
    volumes:
      - type: bind
        source: ../DO_NOT_DELETE_postgres_data
        target: /var/lib/postgresql/data
    environment:
      POSTGRES_DB: gauranga
      POSTGRES_USER: simha
      POSTGRES_PASSWORD: krishna
      PGDATA: "/var/lib/postgresql/data/pgdata"
    networks:
      - postgresql_network
```

# Install the PostgreSQL Django adapter, psycopg2

```
(app) simha@0cc1dc3a7606:~/app$ pipenv install psycopg2
Installing psycopg2...
Resolving psycopg2...
Installing...
Adding psycopg2 to Pipfile's [packages] ...
✔ Installation Succeeded
Pipfile.lock (ee370f) out of date, updating to (1296a7)...
Locking [packages] dependencies...
Building requirements...
Resolving dependencies...
✔ Success!
Locking [dev-packages] dependencies...
Updated Pipfile.lock (eba4dd9a9df378e67521ae2a602674bd4e4519549dde71837cccea498b1296a7)!
Installing dependencies from Pipfile.lock (1296a7)...
(app) simha@0cc1dc3a7606:~/app$

(app) simha@0cc1dc3a7606:~/app$ cat Pipfile
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
django = "==4.1"
psycopg2 = "*"

[dev-packages]

[requires]
python_version = "3.10"


-- Pipfile fix the version of psycopg2 (check the version from Pipfile.lock)


[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
django = "==4.1"
psycopg2 = "==2.9.5"

[dev-packages]

[requires]
python_version = "3.10"


```

# add the postgresql server details in the django settings.py

```
# Database
# https://docs.djangoproject.com/en/4.1/ref/settings/#databases

# DATABASES = {
#     'default': {
#         'ENGINE': 'django.db.backends.sqlite3',
#         'NAME': BASE_DIR / 'db.sqlite3',
#     }
# }

# change the NAME, USER, PASSWORD AND HOST the way one needs
# host name will be postgresql (service name from the docker compose)
DATABASES = {
    "default": {
        "ENGINE": "django.db.backends.postgresql_psycopg2",
        "NAME": "gauranga",
        "USER": "simha",
        "PASSWORD": "krishna",
        "HOST": "postgresql",
        "PORT": "5432",
    }
}
```

# SETTING UP REDIS

# Install redis server

-- redis server is installed as a container using docker compose

```
  redis:
    image: "redis:5.0.9-alpine3.11"
    command: redis-server
    environment:
      - REDIS_REPLICATION_MODE=master
    networks: # connect to the bridge
      - redis_network
```

# install the python package

```
(app) simha@0cc1dc3a7606:~/app$ pipenv install redis
Installing redis...
Resolving redis...
Installing...
Adding redis to Pipfile's [packages] ...
✔ Installation Succeeded
Pipfile.lock (1296a7) out of date, updating to (b3882c)...
Locking [packages] dependencies...
Building requirements...
Resolving dependencies...
✔ Success!
Locking [dev-packages] dependencies...
Updated Pipfile.lock (7159094fd42bd50893ee38930e9d268bb4c68377f934a7415cde279e51b3882c)!
Installing dependencies from Pipfile.lock (b3882c)...

(app) simha@0cc1dc3a7606:~/app$ cat Pipfile
[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
django = "==4.1"
psycopg2 = "*"
redis = "*"

[dev-packages]

[requires]
python_version = "3.10"

-- Pipfile fix the version of redis (check the version from Pipfile.lock)

[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
django = "==4.1"
psycopg2 = "==2.9.5"
redis = "==4.5.1"

[dev-packages]

[requires]
python_version = "3.10"

```

# add the redis server details inside django settings.py

```
# REDIS_HOST
REDIS_HOST = "redis"
REDIS_PORT = "6379"
```

# SETTING UP CELERY

# Installing CELERY

```
(app) simha@0cc1dc3a7606:~/app$ pipenv install celery
Installing celery...
Resolving celery...
Installing...
Adding celery to Pipfile's [packages] ...
✔ Installation Succeeded
Pipfile.lock (b3882c) out of date, updating to (4979f7)...
Locking [packages] dependencies...
Building requirements...
Resolving dependencies...
✔ Success!
Locking [dev-packages] dependencies...
Updated Pipfile.lock (85f636a1fce4539b6a91f41c14006beaae4906e9e0b6fa46855aa0c2a14979f7)!
Installing dependencies from Pipfile.lock (4979f7)...
(app) simha@0cc1dc3a7606:~/app$ 


-- Pipfile

[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
django = "==4.1"
psycopg2 = "*"
redis = "==4.5.1"
celery = "*"

[dev-packages]

[requires]
python_version = "3.10"

-- Pipfile fix the version of celery (check the version of celery from Pipfile.lock)

[[source]]
url = "https://pypi.org/simple"
verify_ssl = true
name = "pypi"

[packages]
django = "==4.1"
psycopg2 = "==2.9.5"
redis = "==4.5.1"
celery = "==5.2.7"

[dev-packages]

[requires]
python_version = "3.10"


```

# Celery server is run a container using docker compose

```
  celery_worker:
    image: "python3.10.10_custom_build:latest"
    volumes:
      - type: bind
        source: ./CODE_FOLDERS/python_django_rest_api
        target: /home/simha/app
    command:
      - sh
      - -c
      - |
        pipenv run celery -app=petsproject worker --loglevel=debug #ensure redis-server is running in root and change backed to respective
    depends_on: # wait for postgresql, redis to be "ready" before starting this service
      - redis
      - postgresql
    networks: # connect to the bridge
      - redis_network
      - postgresql_network



Take note of celery --app=petsproject worker --loglevel=debug:

celery worker is used to start a Celery worker
--app=petsproject runs the petsproject Celery Application (which we'll define shortly)
--loglevel=debug sets the logging level to debug
```

# configuring celery

-- settings.py

```
# REDIS_HOST
REDIS_HOST = "redis"
REDIS_PORT = "6379"

# celery settings
CELERY_BROKER_URL = f"redis://{REDIS_HOST}:{REDIS_PORT}"
CELERY_RESULT_BACKEND = f"redis://{REDIS_HOST}:{REDIS_PORT}"
```

-- add celery.py inside the petsproject folder
-- https://testdriven.io/blog/django-and-celery/

```
import os

from celery import Celery

os.environ.setdefault("DJANGO_SETTINGS_MODULE", "petsproject.settings")

app = Celery("petsproject")

app.config_from_object("django.conf:settings", namespace="CELERY")

app.autodiscover_tasks()


@app.task(bind=True)
def debug_task(self):
    print(f"Request: {self.request!r}")

```

```
What's happening here?

First, we set a default value for the DJANGO_SETTINGS_MODULE environment variable so that Celery will know how to find the Django project.

Next, we created a new Celery instance, with the name core, and assigned the value to a variable called app.

We then loaded the celery configuration values from the settings object from django.conf. We used namespace="CELERY" to prevent clashes with other Django settings. 

All config settings for Celery must be prefixed with CELERY_, in other words.

Finally, app.autodiscover_tasks() tells Celery to look for Celery tasks from applications defined in settings.INSTALLED_APPS.
```

# Update petsprojecy/__init__.py so that the Celery app is automatically imported when Django starts:

```
from .celery import app as celery_app


__all__ = ("celery_app",)
```

# after this setup the docker compose


```
# my version of docker
# $ docker --version
# Docker version 20.10.12, build 20.10.12-0ubuntu2~20.04.1

# my docker compose version
# $ docker-compose --version
# docker-compose version 1.29.2, build 5becea4c


# Compose file format supported till version  19.03.0+ is 3.8
version: "3.7"
services:
  postgresql:
    image: "postgres:13-alpine"
    restart: always
    volumes:
      - type: bind
        source: ../DO_NOT_DELETE_postgres_data
        target: /var/lib/postgresql/data
    environment:
      POSTGRES_DB: gauranga
      POSTGRES_USER: simha
      POSTGRES_PASSWORD: krishna
      PGDATA: "/var/lib/postgresql/data/pgdata"
    networks:
      - postgresql_network

  phppgadmin:
    image: "bitnami/phppgadmin:7.13.0"
    ports:
      - 8026:8080
    environment:
      DATABASE_HOST: "postgresql"
      DATABASE_SSL_MODE: "disable"
    networks:
      - nginx_network
      - postgresql_network

  redis:
    image: "redis:5.0.9-alpine3.11"
    command: redis-server
    environment:
      - REDIS_REPLICATION_MODE=master
    networks: # connect to the bridge
      - redis_network

  celery_worker:
    image: "python3.10.10_custom_build:latest"
    volumes:
      - type: bind
        source: ./CODE_FOLDERS/python_django_rest_api
        target: /home/simha/app
    command:
      - sh
      - -c
      - |
        pipenv run celery --app=petsproject worker --loglevel=debug #ensure redis-server is running in root and change backed to respective
    depends_on: # wait for postgresql, redis to be "ready" before starting this service
      - redis
      - postgresql
    networks: # connect to the bridge
      - redis_network
      - postgresql_network

  webapp:
    image: "python3.10.10_custom_build:latest"
    ports:
      - 8025:8000
    volumes:
      - type: bind
        source: ./CODE_FOLDERS/python_django_rest_api
        target: /home/simha/app
    command:
      - sh
      - -c
      - |
        pipenv run python manage.py runserver 0.0.0.0:8000
    depends_on:
      - postgresql
    stdin_open: true # Add this line into your service
    tty: true # Add this line into your service
    networks:
      - postgresql_network
      - nginx_network

networks:
  postgresql_network:
    driver: bridge
  redis_network:
    driver: bridge
  nginx_network:
    driver: bridge

```

# RUN DOCKER COMPOSE

```
cd /home/extra/simha/django_reactjs_project/docker_setup_with_code_github_repo
docker-compose -p petsproject -f docker-compose-localhost-localdb.yml up --build --force-recreate --remove-orphans
```


# ALL WORKS GOOD


# Migrations

https://docs.djangoproject.com/en/3.2/topics/auth/customizing/#using-a-custom-user-model-when-starting-a-project

Before doing the first migration Extend user to AbstractUser