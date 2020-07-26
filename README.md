# Create a virtual environment with pipenv
mkdir test_django
cd test_django
pipenv install django==2.2.4 gunicorn --python 3.6
pipenv shell

# Create a Django project
django-admin startproject project .

# Local DB & Migrations
python manage.py makemigrations
python manage.py migrate
python manage.py createsuperuser

# Update Django settings.py

# DEBUG can be True/False or 1/0
DEBUG = int(os.environ.get('DEBUG', default=1)) 

# Create .env
echo DEBUG=1 >> .env

# Test Gunicorn
gunicorn cfehome.wsgi:application --bind 0.0.0.0:8000


# Docker 
touch Dockerfile

# Build your docker image
docker build -t docker-django -f Dockerfile .

# Run your container
docker run -it --name django -p 8000:8000 docker-django
