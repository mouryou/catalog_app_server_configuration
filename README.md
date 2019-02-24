# The server configuration of catalog_app

## IP Address and SSH port

The IP address is `3.83.109.131` but you may need to use `http://3-83-109-131.nip.io/` to login with Google account. The SSH port is 2200.

## Complete URL

The complete URL is `http://3-83-109-131.nip.io/`.

## Software installed and configuration made

Apache2 is installed to establish the HTTP server, and mod_wsgi is installed to connect Apache and Flask. To run the application, Flask and some dependencies such as SQLAlchemy, oauth2client, and werkzeug are installed. I use Sqlite as the database for the catalog app.

For Amazon Lightsail, I configure the networking configuration to only allow connections through ports 80, 123 and 2200.

In the instance, all packages are updated by `sudo apt-get update` and `sudo apt-get upgrade`. `ufw` is enabled and set to only allow connections from ports 80, 123 and 2200.

In `/etc/ssh/sshd_config`, `PermitRootLogin` is set to `no` to prevent root logged in remotely. `Port` for SSH is changed to 2200, and `PasswordAuthentication` is set to `no` to enforce users to use RSA keys to ssh into the instance.

A new user `grader` is created for the reviewer, and a key pair is generated on the local machine and then the public key is set for `grader` in the instance. Also `grader` is configured to be able to use `sudo` command.

And in `/etc/apache2/sites-enabled`, these codes

```conf
        WSGIDaemonProcess catalog_app python-path=/var/www/catalog_app:/var/www/catalog_app/venv/lib/python2.7/site-packages
        WSGIScriptAlias / /var/www/catalog_app/catalog_app.wsgi

        <Directory /var/www/catalog_app>
            WSGIProcessGroup catalog_app
            WSGIApplicationGroup %{GLOBAL}
            Order deny,allow
            Allow from all
        </Directory>
```

are added to run the application on the server.

## Third-party resources

[Flask Hello World App with Apache WSGI](https://www.bogotobogo.com/python/Flask/Python_Flask_HelloWorld_App_with_Apache_WSGI_Ubuntu14.php)  
[mod_wsgi (Apache)](http://flask.pocoo.org/docs/1.0/deploying/mod_wsgi/)  
[Flask Documentation](http://flask.pocoo.org/docs/1.0/)
