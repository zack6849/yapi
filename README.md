# YAPI
YAPI is a local docker development environment for PHP applications,
It's built with laravel in mind, but any old PHP application should run OK on it

Services setup OOTB:
 - Nginx w/ PHP7
 - MariaDB
 - MailHog
 - Redis
 - Selenium hub with a chrome node (for browser automation testing)

# Use
To use YAPI, you'll want to add this as a submodule to your git project, eg:
```
git submodule add git@github.com:zack6849/yapi.git
```
If you're using a unix system and your UID and GID aren't both 1000, create an .env file in this directory, and give it the following contents:
```shell
PUID=<YOUR UID>
PGID=<YOUR GID>
```

Then, to start the machine, just run the relevant script for your platform (start.sh for *nix systems, start.bat for windows)

### MySQL Credentials:
```
Host: db
Username: root
Password: secret
Database: laravel
```

### MailHog
MailHog is accessible through http://mailhog:8025/ on the host machine, and will show any emails sent from your application

### Example .env settings for a standard laravel application:
```
DB_CONNECTION=mysql
DB_HOST=db
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=root
DB_PASSWORD=secret

REDIS_HOST=redis
REDIS_PASSWORD=null
REDIS_PORT=6379

MAIL_DRIVER=smtp
MAIL_HOST=mailhog
MAIL_PORT=1025
MAIL_USERNAME=null
MAIL_PASSWORD=null
MAIL_ENCRYPTION=null
```

# Enabling Xdebug
To enable xdebug, uncomment the relevant lines in php/Dockerfile
