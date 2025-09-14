# Task: Integrate PHP With Keycloak and perform SSO with Keycloak Credentials
---
## Table of Contents
- [PHP Installation](#1-php-installation)
- [Integrating with Keycloak SSO](#2-integrating-with-keycloak-sso)
- [Performing SSO with Keycloak](#3-performing-sso-with-keycloak)

PHP APPLICATION: http://139.59.20.222/php/ 

---
## 1. PHP Installation
- First, we'll create a dedicated directory for your new PHP application's files.
- Install the PHP OIDC Library
```bash
    sudo mkdir /var/www/phpapp
    sudo composer require jumbojett/openid-connect-php
```

![0](./images/5/0.png)

- Create the following files:

1. index.php

```bash
    sudo nano /var/www/phpapp/index.php
```
- Acts as the main entry point
- It checks the PHP session to see if a user is already logged in.

![1](./images/5/1.png)

2. login.php

```bash
    sudo nano /var/www/phpapp/login.php
```
- Initiates authentication
- Redirects to Keycloak
  
![2](./images/5/2.png)

3. callback.php

```bash
    sudo nano /var/www/phpapp/callback.php
```
- Receives data from Keycloak
- Authenticates the user
- Establishes a local session

![3](./images/5/3.png)

4. logout.php

```bash
    sudo nano /var/www/phpapp/logout.php
```
- Initiates a global logout
- Redirects to the homepage 

![4](./images/5/4.png)

- Next- Edit applications.conf of Httpd service

![applications-conf](./images/4/applications-conf.png)

---

## 2. Integrating with Keycloak SSO

- We visit our keycloak administration console
- Then we create a new client - php-app

![5](./images/5/5.png)


- We put valid redirect URLs and copy client credentials

![6](./images/5/6.png)

- Paste Client Credentials in callback.php

```php
$oidc = new OpenIDConnectClient(
        'https://139.59.20.222/realms/master',
        'php-app',
        'client_secret_from_keycloak' 
    );
```
---

## 3. Performing SSO with Keycloak

Go to PHP application http://139.59.20.222/php/

- Click login

![7](./images/5/7.png)

- Redirected to Keycloak admin

![8](./images/5/8.png)

- Once we login we can see out php homepage

![9](./images/5/9.png)
