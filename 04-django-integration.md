# Task: Integrate Django With Keycloak and perform SSO with Keycloak Credentials
---
## Table of Contents
- [Django Installation](#1-django-installation)
- [Integrating with Keycloak SSO](#2-integrating-with-keycloak-sso)
- [Performing SSO with Keycloak](#3-performing-sso-with-keycloak)

---

## 1. Django Installation

![1](./images/4/1.png)

![2](./images/4/2.png)

![3](./images/4/3.png)

Editing the settings python file

```python
# Database
#https://docs.djangoproject.com/en/5.2/ref/settings/ 
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'djangodb',
        'USER': 'djangouser',
        'PASSWORD': 'your_passoword',
        'HOST': 'localhost',
        'PORT':'3306',
        }
    }
```

```python
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [BASE_DIR / 'templates'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
                ],
            },
    },
]                                                                                                                                 
```
```python
STATIC_URL = '/django/static/'
STATIC_ROOT = BASE_DIR / 'static'
```
```python
AUTHENTICATION_BACKENDS = (
    'mozilla_django_oidc.auth.OIDCAuthenticationBackend',
    'django.contrib.auth.backends.ModelBackend',
)                                                
OIDC_RP_CLIENT_ID = "django"                 
OIDC_RP_CLIENT_SECRET = "client_secret"
OIDC_OP_AUTHORIZATION_ENDPOINT = "https://139.59.20.222/realms/master/protocol/openid-connect/auth"
OIDC_OP_TOKEN_ENDPOINT = "https://139.59.20.222/realms/master/protocol/openid-connect/token"
OIDC_OP_USER_ENDPOINT = "https://139.59.20.222/realms/master/protocol/openid"
OIDC_OP_JWKS_ENDPOINT = "https://139.59.20.222/realms/master/protocol/openid-connect/certs"
OIDC_VERIFY_SSL = False  
LOGIN_REDIRECT_URL = "/django/home"
LOGOUT_REDIRECT_URL = "/"  
FORCE_SCRIPT_NAME = '/django'                                               
```

![4](./images/4/4.png)

Next we create superuser in our venv

![5](./images/4/5.png)

We edit /etc/systemd/system/gunicorn.service

![6](./images/4/6.png)

We also edit 

1. Urls.py

```python
from django.contrib import admin
from .views import login_page, home_page
from django.urls import path, include
urlpatterns = [
    path('admin/', admin.site.urls),
    path('oidc/', include('mozilla_django_oidc.urls')),
    path('', login_page, name='login'),
    path('home/', home_page, name='home')
    ] 
```

2. views.py

```python
from django.shortcuts import render
from django.contrib.auth.decorators import login_required
def login_page(request):return render(request, 'login.html')@login_required
def home_page(request):                         
#After a successful OIDC login, user details are in request.user
user_info = {
'last_name': request.user.last_name,
'email': request.user.email,
'username': request.user.username,} 
# You can render a template or just return a simple response                                                             
return render(request, 'home.html', {'user_info': user_info})
```

Next we edit /etc/httpd/conf.d/applications.conf

![applications-conf](./images/4/applications-conf.png)

We make a templates directory and code out a home and a login HTML page

![8.1](./images/4/8.1.png)

We collect static files
We run
```bash
python manage.py collectstatic
```
This collects all the static files in our project so they can load faster

We then enable and run gunicorm service

![7](./images/4/7.png)

---

## 2. Integrating with Keycloak SSO

Login to our keycloak console  
Create a new user 

![9](./images/4/9.png)

We copy client secret key and paste it in settings.py file
```python
OIDC_RP_CLIENT_SECRET = "client_secret"
```

Next we put a valid redirect url and create user

![10](./images/4/10.png)

We Restart gunicorn service and change ownership to apache user

![11](./images/4/11.png)

---

## 3. Performing SSO with Keycloak

We go to to our login page http://139.59.20.222/django

![12](./images/4/12.png)

Once we click on login with Keycloak, we are redirected to keycloak login page  
We put in our testuser credentials which we had created in keycloak

![13](./images/4/13.png)

We are successfully redirected to django/home/ page  
Where you can see testuser username and email as we directed in home.html file

![14](./images/4/14.png)

