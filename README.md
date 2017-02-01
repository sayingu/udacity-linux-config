1. The IP address and SSH port so your server can be accessed by the reviewer.
  * 52.33.111.203 -p 2200

2. The complete URL to your hosted web application.
  * http://ec2-52-33-111-203.us-west-2.compute.amazonaws.com/

3. A summary of software you installed and configuration changes made.
  * apache2
  * libapache2-mod-wsgi
    * change /etc/apache2/sites-enabled/000-default.conf file for my application
  * postgresql
    * change /etc/postgresql/9.3/main/pg_hba.conf file for user "catalog"
  * python-dev, libpq-dev (for psycopg2 install)
  * git
  * python-virtualenv
    * sqlalchemy, psycopg2 (in the virtual env)

4. A list of any third-party resources you made use of to complete this project.
  * I basically read the articles at last lines in Getting Started Guide.
And many times googling the keyword of errors.
