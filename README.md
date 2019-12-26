# YAPI
Yet another PHP Image.

# Use
You'll want to add this as a submodule to your project, eg:

```
git submodule add git@github.com:zack6849/yapi.git
```

You'll also want to make sure to define a host of laravel.local in your hosts file that maps to your local machine.
If you're using a unix system and your UID and GID aren't both 1000, create a .env file in this directory, and give it the following contents:

```shell
PUID=<YOUR UID>
PGID=<YOUR GID>
```

Then, to start the machine, just run
```shell
$ docker compose up -d
```

Once you're all set, the following are the credentials:


### MySQL:
Host: db
Username: root
Password: secret
Database: laravel

### MailHog
MailHog is accessible through http://localhost:8025/ and will show any emails sent from your application

