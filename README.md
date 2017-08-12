# How to set up Djanjo with PostgreSQL on Ubuntu 16.04

## Setup Ubuntu 16.04
- Either follow [this guide](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04) to do it properly on a remove server OR
- Pick box from [Vagrant](https://app.vagrantup.com/boxes/)
- I chose [ubuntu/xenial64](https://app.vagrantup.com/ubuntu/boxes/xenial64)
  - Create & navigate to new folder in terminal (I'm calling mine `DjP` cloning it from an empty GitHub & copying in the .gitignore from my [DjangoPostgres](https://github.com/Aqueum/DjangoPostgres/blob/master/.gitignore))
  - `vagrant init ubuntu/xenial64`
  - edit vagrant file
    - change `# config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"`
    - to `config.vm.network "forwarded_port", guest: 8000, host: 8000, host_ip: "127.0.0.1"`
  - `vagrant up` if first time or `vagrant reload` to load the new vagrant file
  - `vagrant box update` to get latest version
  - `vagrant ssh` to SSH into your new box
  - `cd /vagrant` to get into the shared diectory

the following is from [how-to-use-postgresql-with-your-django-application-on-ubuntu-16-04](https://www.digitalocean.com/community/tutorials/how-to-use-postgresql-with-your-django-application-on-ubuntu-16-04):
## Install the Components from the Ubuntu Repositories 
- `sudo apt-get update`
- `sudo apt-get install python3-pip python3-dev libpq-dev postgresql postgresql-contrib`

## Create a Database and Database User
- `sudo -u postgres psql` to log in to PostgreSQL as postgres
- enter:
```
CREATE DATABASE myproject;
CREATE USER myprojectuser WITH PASSWORD 'password';
ALTER ROLE myprojectuser SET client_encoding TO 'utf8';
ALTER ROLE myprojectuser SET default_transaction_isolation TO 'read committed';
ALTER ROLE myprojectuser SET timezone TO 'UTC';
GRANT ALL PRIVILEGES ON DATABASE myproject TO myprojectuser;
```
- `\q` to exit PostgreSQL

## Install Django within a Virtual Environment
- `sudo -H pip3 install --upgrade pip`(because it always complains)
- `sudo -H pip3 install virtualenv` (added the -H at the prompts suggestion)
- `mkdir /vagrant/myproject`
- `cd /vagrant/myproject`
- `virtualenv myprojectenv` to create virtual environment
- `source myprojectenv/bin/activate`
- `pip install django psycopg2`
- `django-admin.py startproject myproject .`

## Configure the Django Database Settings
- edit `DATABASES` in `myproject/myproject/settings.py` to:
```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.postgresql_psycopg2',
        'NAME': 'myproject',
        'USER': 'myprojectuser',
        'PASSWORD': 'password',
        'HOST': 'localhost',
        'PORT': '',
    }
}
```
- edit `ALLOWED_HOSTS` in `myproject/myproject/settings.py` to: `ALLOWED_HOSTS = ['.localhost', '35.176.170.23', '192.168.1.65', '109.157.214.104', '127.0.0.1']` these are: local host, my lightsail ip, mu local host according to system, my public ip, standard localhost

## Migrate the Database and Test Project
- `cd /vagrant/myproject`
- `python manage.py makemigrations`
- `python manage.py migrate`
- `python manage.py createsuperuser` I used myprojectuser & password2
- `python manage.py runserver 0.0.0.0:8000`
- [localhost:8000](http://localhost:8000/)
- [localhost:8000/admin](http://localhost:8000/admin)

End of digitalocean setup tutorial, the following is based on [django polls tutorial](https://docs.djangoproject.com/en/1.11/intro/tutorial01/)

## Create app
- ctrl-c to exit server
- `cd /vagrant/myproject` to get to folder with manage.py
- `python manage.py startapp polls` to create polls app starter

## Add a view
- edit `polls/views.py` to: 













