1. Create a new user named grader

> useradd grader

> passwd grader


2. Give the grader the permission to sudo
> visudo

  * Defaults timestamp_timeout=15
  * grader ALL=(ALL:ALL) ALL

* Set grader can connected by ssh

> cd /home/grader

> ssh-keygen

> cp id_rsa authorized_keys

> vi /etc/ssh/ssh_config
  * AuthorizedKeysFile      %h/.ssh/authorized_keys

* disable root connection
  * PermitRootLogin no


3. Update all currently installed packages

> apt-get update


4. Change the SSH port from 22 to 2200

> netstat -tulpn |grep sshd
> vi /etc/ssh/sshd_config
> service ssh restart


5. Configure the Uncomplicated Firewall (UFW) to only allow incoming connections for SSH (port 2200), HTTP (port 80), and NTP (port 123)

> ufw status

> ufw default deny incoming

> ufw default allow outgoing

> ufw allow 2200/tcp

> ufw allow 80/tcp

> ufw allow 123/tcp

> ufw disable

> ufw enable


6. Configure the local timezone to UTC

> date

> ln -sf /usr/share/zoneinfo/UTC /etc/localtime


7. Install and configure Apache to serve a Python mod_wsgi application

> apt-get install apache2

> apt-get install libapache2-mod-wsgi

* check html http://ec2-35-166-251-196.us-west-2.compute.amazonaws.com/

> vi /etc/apache2/sites-enabled/000-default.conf

ServerName catalog

WSGIScriptAlias / /var/www/udacity-catalog/catalog.wsgi
<Directory /usr/share/git/udacity-catalog>
    Order deny,allow
    Allow from all
</Directory>
<Directory "^/.*/\.git/">
    Order deny,allow
    Deny from all
</Directory>

> service apache2 restart


8. Install and configure PostgreSQL:
Do not allow remote connections
Create a new user named catalog that has limited permissions to your catalog application database

> apt-get install postgresql

> apt-get install python-dev

> apt-get install libpq-dev

> sudo -u postgres createuser -D -A -P catalog

> sudo -u postgres createdb -O catalog catalog

> /etc/init.d/postgresql restart

> vi /etc/postgresql/9.3/main/pg_hba.conf

* local catalog catalog ident


9. Install git, clone and setup your Catalog App project (from your GitHub repository from earlier in the Nanodegree program) so that it functions correctly when visiting your server’s IP address in a browser. Remember to set this up appropriately so that your .git directory is not publicly accessible via a browser!

> apt-get install git

> cd /var/www/

> git clone https://github.com/sayingu/udacity-catalog.git

> apt-get install python-virtualenv

> . venv/bin/activate

> pip install sqlalchemy

> pip install psycopg2

> python database_init.py

> tail -f /var/log/apache2/error.log
