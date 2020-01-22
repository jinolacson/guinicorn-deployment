# Deploy with WSGI & Gunicorn & Deploying Static Files


[Web Server Gateway Interface (WSGI)](https://django.readthedocs.io/en/1.5.x/howto/deployment/wsgi/index.html), the Python standard for web servers and applications. Django's startproject management command sets up a simple default WSGI configuration for you, which you can tweak as needed for your project, and direct any WSGI-compliant webserver to use


### Running django project using development server


1.) Install django.


```
dev@jino:~/sample-app$ pipenv install django
Creating a virtualenv for this project…
Pipfile: /home/dev/sample-app/Pipfile
Using /usr/bin/python3 (3.6.9) to create virtualenv…
⠴ Creating virtual environment...Already using interpreter /usr/bin/python3
Using base prefix '/usr'
New python executable in /home/dev/.local/share/virtualenvs/sample-app-3nWZ8UIv/bin/python3
Also creating executable in /home/dev/.local/share/virtualenvs/sample-app-3nWZ8UIv/bin/python
Installing setuptools, pip, wheel...
done.
✔ Successfully created virtual environment! 
Virtualenv location: /home/dev/.local/share/virtualenvs/sample-app-3nWZ8UIv
Creating a Pipfile for this project…
Installing django…
Adding django to Pipfile's [packages]…
✔ Installation Succeeded 
Pipfile.lock not found, creating…
Locking [dev-packages] dependencies…
Locking [packages] dependencies…
✔ Success! 
Updated Pipfile.lock (85c883)!
Installing dependencies from Pipfile.lock (85c883)…
  🐍   ▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉ 4/4 — 00:00:02
To activate this project's virtualenv, run pipenv shell.
Alternatively, run a command inside the virtualenv with pipenv run.
```

2. Activate virtual environment.


```
dev@jino:~/sample-app$ pipenv shell
Launching subshell in virtual environment…
 . /home/dev/.local/share/virtualenvs/sample-app-3nWZ8UIv/bin/activate
dev@jino:~/sample-app$  . /home/dev/.local/share/virtualenvs/sample-app-3nWZ8UIv/bin/activate
```

3. Start new project called `myproject`.


```
(sample-app) dev@jino:~/sample-app$ django-admin startproject myproject
(sample-app) dev@jino:~/sample-app$ cd myproject
(sample-app) dev@jino:~/sample-app/myproject$ ls
manage.py  myproject
```

4. Migrate default tables.


```
(sample-app) dev@jino:~/sample-app/myproject$ python3 manage.py migrate
Operations to perform:
  Apply all migrations: admin, auth, contenttypes, sessions
Running migrations:
  Applying contenttypes.0001_initial... OK
  Applying auth.0001_initial... OK
  Applying admin.0001_initial... OK
  Applying admin.0002_logentry_remove_auto_add... OK
  Applying admin.0003_logentry_add_action_flag_choices... OK
  Applying contenttypes.0002_remove_content_type_name... OK
  Applying auth.0002_alter_permission_name_max_length... OK
  Applying auth.0003_alter_user_email_max_length... OK
  Applying auth.0004_alter_user_username_opts... OK
  Applying auth.0005_alter_user_last_login_null... OK
  Applying auth.0006_require_contenttypes_0002... OK
  Applying auth.0007_alter_validators_add_error_messages... OK
  Applying auth.0008_alter_user_username_max_length... OK
  Applying auth.0009_alter_user_last_name_max_length... OK
  Applying auth.0010_alter_group_name_max_length... OK
  Applying auth.0011_update_proxy_permissions... OK
  Applying sessions.0001_initial... OK
```

5. Host in our development server.


```
(sample-app) dev@jino:~/sample-app/myproject$ ./manage.py runserver
Watching for file changes with StatReloader
Performing system checks...
System check identified no issues (0 silenced).
January 22, 2020 - 06:48:43
Django version 3.0.2, using settings 'myproject.settings'
Starting development server at http://127.0.0.1:8000/
Quit the server with CONTROL-C.
```


### Running django project using gunicorn

1. Install [gunicorn](https://django.readthedocs.io/en/1.5.x/howto/deployment/wsgi/gunicorn.html) using `pip`.

```
(sample-app) dev@jino:~/sample-app/myproject$ pipenv install gunicorn
Installing gunicorn…
Adding gunicorn to Pipfile's [packages]…
✔ Installation Succeeded 
Pipfile.lock (a8faad) out of date, updating to (85c883)…
Locking [dev-packages] dependencies…
Locking [packages] dependencies…
✔ Success! 
Updated Pipfile.lock (a8faad)!
Installing dependencies from Pipfile.lock (a8faad)…
  🐍   ▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉▉ 5/5 — 00
```

2. Host `wsgi.py` file .


```
(sample-app) dev@jino:~/sample-app/myproject$ gunicorn myproject.wsgi
[2020-01-22 14:52:25 +0800] [18734] [INFO] Starting gunicorn 20.0.4
[2020-01-22 14:52:25 +0800] [18734] [INFO] Listening at: http://127.0.0.1:8000 (18734)
[2020-01-22 14:52:25 +0800] [18734] [INFO] Using worker: sync
[2020-01-22 14:52:25 +0800] [18737] [INFO] Booting worker with pid: 18737
Not Found: /static/admin/css/fonts.css
Not Found: /favicon.ico
```

After running your app with gunicorn, go to the django admin panel at `localhost:8000/admin`. You will see that all styles are gone. The reason is that gunicorn is an application server and **it does not serve static files**. In order to solve this problem, we will take a look at [Nginx next](https://rahmonov.me/posts/run-a-django-app-with-nginx-and-gunicorn/) and use it as a reverse proxy for gunicorn. We will talk about what reverse proxy is as well so don't think about it for now.


### Resources

- [docs.gunicorn.org](http://docs.gunicorn.org/en/stable/)

- [Green unicorn in ubuntu](https://rahmonov.me/posts/run-a-django-app-with-gunicorn-in-ubuntu-16-04/)

- [Deploying Static Files](https://docs.djangoproject.com/en/3.0/howto/static-files/deployment/)

