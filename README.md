This repository documents my implementation of the SSO project with Keycloak with 3 web apps. All setup steps, configurations, and screenshots are included.

-----

# Keycloak SSO Integration for Drupal, Django, and PHP

### Deployed Applications 

| Application | URL | Key Technologies |
| :--- | :--- | :--- |
| **Keycloak** | https://139.59.20.222/admin | Java, Apache (Reverse Proxy) |
| **Drupal** | http://139.59.20.222/drupal/ | PHP, Composer, MariaDB |
| **Django** | http://139.59.20.222/django/ | Python, Django, Gunicorn |
| **PHP App** | http://139.59.20.222/php/ | PHP, Composer |

[NOTE]: These links pointed to a temporary test server, which is no longer running. 

## Overview

* This repository provides a complete, step-by-step guide for setting up a full Single Sign-On (SSO) environment using Keycloak on a Rocky Linux 10 server.  
* The documentation covers the entire process, from initial server configuration to integrating three different web applications: Drupal, Django, and a vanilla PHP application.

## Table of Contents

1.  **[01-server-setup.md](https://github.com/Aaryan-khairnar/my-internship-tasks-FOSSEE/blob/main/01-server-setup.md)**

      * Initial Digital Ocean droplet creation
      * Basic security hardening (creating a sudo user, disabling root SSH)
      * Firewall configuration, and installation of core system packages like Apache, MariaDB, and PHP.

2.  **[02-keycloak-setup.md](https://github.com/Aaryan-khairnar/my-internship-tasks-FOSSEE/blob/main/02-keycloak-setup.md)**

      * Installation and initial setup of Keycloak in **development mode**. 
      * This includes installing Java, configuring a dedicated Keycloak user, handling SELinux contexts, and performing the first login to the admin console.

3.  **[02.5-keycloak-production-setup.md](https://github.com/Aaryan-khairnar/my-internship-tasks-FOSSEE/blob/main/02.5-keycloak-production-setup.md)**

      * Transitioning the Keycloak instance to a **production-ready setup**. 
      * This guide covers migrating the database from H2 to MariaDB
      * Configuring Apache as a reverse proxy for SSL/TLS termination
      * Running Keycloak as a `systemd` service.

4.  **[03-drupal-integration.md](https://github.com/Aaryan-khairnar/my-internship-tasks-FOSSEE/blob/main/03-drupal-integration.md)**

      * Installing a Drupal site from scratch and integrating it with Keycloak for SSO using the OpenID Connect module.

5.  **[04-django-integration.md](https://github.com/Aaryan-khairnar/my-internship-tasks-FOSSEE/blob/main/04-django-integration.md)**

      * Setting up a Python Django project, configuring it to run with Gunicorn, and integrating it with Keycloak for SSO.

6.  **[05-php-integration.md](https://github.com/Aaryan-khairnar/my-internship-tasks-FOSSEE/blob/main/05-php-integration.md)**

      * Creating a simple, custom PHP application and integrating it with Keycloak for SSO.

## Technology Stack

  * **Operating System**: Rocky Linux 10
  * **Identity & Access Management**: Keycloak
  * **Web Server**: Apache (httpd)
  * **Database**: MariaDB
  * **Applications**:
      * Drupal
      * Django (with Gunicorn)
      * PHP

## Other Features 

* Open User Registration: 

To fully demonstrate the SSO workflow, the Keycloak master realm has been configured to permit self-service user registration. 

This feature allows public users to create their own accounts, which can then be used to authenticate against the integrated Drupal, Django, and PHP applications. (Only until the Digitalocean droplet is alive and running)
