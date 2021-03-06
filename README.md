# HTTP Server Stack (includes Nginx and uWSGI)

* Instructions:

14.04:

[How To Serve Flask Applications with uWSGI and Nginx on Ubuntu 14.04](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-14-04)

16.04:

[How To Serve Flask Applications with uWSGI and Nginx on Ubuntu 16.04](https://www.digitalocean.com/community/tutorials/how-to-serve-flask-applications-with-uwsgi-and-nginx-on-ubuntu-16-04)

# Package Requirements

```
virtualenv __
. __/bin/activate
pip install requirements.txt
```

# Nginx

## Copy Nginx Conf.

```
sudo cp ori_nginx.conf /etc/nginx/sites-available/ori
sudo ln -s /etc/nginx/sites-available/ori /etc/nginx/sites-enabled
```

### Trouble shooting - HTTP 400

* check if all conf has been correctly set

* `ipv6only=on` is must, otherwise it will listen twice for the same port

  * e.g. `listen [::]:443 ssl ipv6only=on default_server;`

* Don't include `ssl on;`

### Trouble shooting - `conflicting server name "xxx.xx" on 0.0.0.0:80, ignored`

* check if `/etc/nginx/sites-available/twitch_analysis~` exists

## Global conf.

```
/etc/nginx/nginx.conf
```

## Log

```
/var/log/nginx/error.log
```

## restart

```
sudo service nginx restart
```

# uWSGI

## Copy uWSGI Upstart Script (14.04)

```
sudo cp twitch_analysis_uwsgi.conf /etc/init/twitch_analysis.conf
sudo start twitch_analysis
```

## Copy `twitch_analysis` uWSGI Systemd Unit File (16.04)

```
sudo cp twitch_analysis_uwsgi.service /etc/systemd/system/twitch_analysis_uwsgi.service
sudo systemctl start twitch_analysis_uwsgi
sudo systemctl enable twitch_analysis_uwsgi
```

### Copy `line_bot` uWSGI Systemd Unit File (16.04)

```
sudo cp line_bot_uwsgi.service /etc/systemd/system/line_bot_uwsgi.service
sudo systemctl start line_bot_uwsgi
sudo systemctl enable line_bot_uwsgi
```

## Log

```
/var/log/uwsgi/uwsgi.log
```

# ACME (Automatic Certificate Management Environment)

[acme-nginx](https://github.com/kshcherban/acme-nginx)

