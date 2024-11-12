# Frappe-ERPNext Installation Guide Cheat Sheet
 Steps to set up Frappe-ERPNext

# Frappe/ERPNext Version-15 in Ubuntu 22.04 LTS Installation Guide
A complete Guide to Install Frappe/ERPNext version 15  in Ubuntu 22.04 LTS

#### Refer this for default python 3.11 setup

- [D-codeE Video Tutorial](https://youtu.be/TReR0I0O1Xo)

### Pre-requisites 

      Python 3.11+
      Node.js 18+
      
      Redis 5                                       (caching and real time updates)
      MariaDB 10.3.x / Postgres 9.5.x               (to run database driven apps)
      yarn 1.12+                                    (js dependency manager)
      pip 20+                                       (py dependency manager)
      wkhtmltopdf (version 0.12.6 with patched qt)  (for pdf generation)
      cron                                          (bench's scheduled jobs: automated certificate renewal, scheduled backups)
      NGINX                                         (proxying multitenant sites in production)


------
### Steps to Install python 3.11.xx
------

> ## `Note: If you are using ubuntu 23.xx or latest  the default python version is 3.11.xx. So you can skip the python 3.11 installation steps`
    
#### First, import the Python repository with the most up-to-date stable releases.

      sudo add-apt-repository ppa:deadsnakes/ppa -y
      sudo apt update
      
#### install Python 3.11 by executing the following command in your terminal:

      sudo apt install python3.11
      python3.11 --version

    
#### To install all the extras in one go, run the following command.

      sudo apt install python3.11-full



#### Refer this for default python 3.11 setup

- [www.linuxcapable.com](https://www.linuxcapable.com/how-to-install-python-3-11-on-ubuntu-linux/#google_vignette)
- [ubuntuhandbook.org](https://ubuntuhandbook.org/index.php/2022/10/python-3-11-released-how-install-ubuntu)

-----


### STEP 1 Install git
    sudo apt-get install git

### STEP 2 install python-dev

    sudo apt-get install python3-dev

### STEP 3 Install setuptools and pip (Python's Package Manager).

    sudo apt-get install python3-setuptools python3-pip

### STEP 4 Install virtualenv
    
    sudo apt install python3.11-venv
    

### STEP 5 Install MariaDB

    sudo apt-get install software-properties-common
    sudo apt install mariadb-server
    sudo mysql_secure_installation
    
    
      In order to log into MariaDB to secure it, we'll need the current
      password for the root user. If you've just installed MariaDB, and
      haven't set the root password yet, you should just press enter here.

      Enter current password for root (enter for none): # PRESS ENTER
      OK, successfully used password, moving on...
      
      
      Switch to unix_socket authentication [Y/n] Y
      Enabled successfully!
      Reloading privilege tables..
       ... Success!
 
      Change the root password? [Y/n] Y
      New password: 
      Re-enter new password: 
      Password updated successfully!
      Reloading privilege tables..
       ... Success!

      Remove anonymous users? [Y/n] Y
       ... Success!
 
       Disallow root login remotely? [Y/n] Y
       ... Success!

       Remove test database and access to it? [Y/n] Y
       - Dropping test database...
       ... Success!
       - Removing privileges on test database...
       ... Success!
 
       Reload privilege tables now? [Y/n] Y
       ... Success!

 
    
    
    
### STEP 6  MySQL database development files

    sudo apt-get install libmysqlclient-dev

### STEP 7 Edit the mariadb configuration ( unicode character encoding )

    sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf

add this to the 50-server.cnf file

    
    [server]
    user = mysql
    pid-file = /run/mysqld/mysqld.pid
    socket = /run/mysqld/mysqld.sock
    basedir = /usr
    datadir = /var/lib/mysql
    tmpdir = /tmp
    lc-messages-dir = /usr/share/mysql
    bind-address = 127.0.0.1
    query_cache_size = 16M
    log_error = /var/log/mysql/error.log
    
    [mysqld]
    innodb-file-format=barracuda
    innodb-file-per-table=1
    innodb-large-prefix=1
    character-set-client-handshake = FALSE
    character-set-server = utf8mb4
    collation-server = utf8mb4_unicode_ci      
     
    [mysql]
    default-character-set = utf8mb4

Now press (Ctrl-X) to exit

    sudo service mysql restart

### STEP 8 install Redis
    
    sudo apt-get install redis-server

### STEP 9 install Node.js 18.X package

    sudo apt install curl 
    curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
    source ~/.profile
    nvm install 18.16.1
    nvm use 18.16.1

### STEP 10  install Yarn

    sudo apt-get install npm

    sudo npm install -g yarn

### STEP 11 sudo apt-get install xvfb libfontconfig wkhtmltopdf

    sudo apt-get install xvfb libfontconfig wkhtmltopdf
    

### STEP 12 install frappe-bench

    sudo -H pip3 install frappe-bench
    
    bench --version

### NOTE: 
      Steps 1-12 can be done in the root. But 13 and below must be done through a user.
      create user: adduser [username]
      su [username] to switch to the user
      sudo visudo in root to add the new user
      from user command, cd ~ and continue.
      
    
### STEP 13 initilise the frappe bench & install frappe latest version 

    bench init frappe-bench --frappe-branch version-15 --python python3.11
    
    cd frappe-bench/
    bench start

    *uninstall if there are issues: rm -rf frappe-bench
    
### STEP 14 create a site in frappe bench 
    
    bench new-site frappe.site.local
    
    bench use frappe.site.local

    * If issues persist, drop the site "bench drop-site frappe.site.local --force", create new and bench update before proceeding to STEP 15.

Open url http://frappe.site.local:8000 to login


### STEP 15 install ERPNext latest version in bench & site

    
    bench get-app erpnext --branch version-15
    ###OR
    bench get-app https://github.com/frappe/erpnext --branch version-15

    bench --site frappe.site.local install-app erpnext
    
    bench start

    ## See error 4 below for issues.


Additional: 
### 15 STEP 16
      Using power toys add new entry in Administrator: Hosts File Editor
            Address: 127.0.0.1
            Host: frappe.site.local (or desired name)
            Then click the ADD button
      Log on ERPNext using Administrator and the password used during the installation.


## Errors
       # Error 1: "Access denied for user 'root'@'localhost'")

              sudo mysql -u root // enter MariaDB monitor
                     GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' WITH GRANT OPTION; 
                     PRIVILEGES;
                     FLUSH PRIVILEGES;
                     EXIT;
                     
      Continue to STEP 14



     #  Error 2:  during step 13
            "The error message indicates that the virtual environment creation failed because ensurepip is not available, and it suggests installing the                   python3-venv package."
            
            sudo apt install python3.10-venv

      # Error 3: during step 14 -  bench new-site frappe.site.local
            Issue with the MariaDB configuration. Specifically, it mentions an unknown variables, i.e. 'pid-file=/run/mysqld/mysqld.pid', 'expire_logs_days=10'\n", etc,\. 
            # To open the main configuration file
                  sudo nano /etc/mysql/my.cnf
      
            # Open this and comment out # all unnecessary lines
                  sudo nano /etc/mysql/mariadb.conf.d/50-server.cnf

      # Error 4: WARN: restart failed: Couldn't find supervisorctl in PATH
            Run the following: 
                  sudo apt-get update
                  sudo apt-get install supervisor

                  Install Supervisor:
                        Ensure Supervisor is installed on your system if it isn't already:
                        
                        sudo apt-get update
                        sudo apt-get install supervisor
                        Verify Supervisor: Check that supervisorctl is in your PATH:
                        
                              which supervisorctl
                        This should return a path like /usr/bin/supervisorctl.
                        
                        Configure Supervisor: Set up Supervisor to manage Frappe/ERPNext processes. Create a configuration file for your Frappe/ERPNext setup if it doesn't exist:
                        
                              sudo nano /etc/supervisor/conf.d/frappe-bench.conf
                              Add the necessary configurations (adjust paths and commands as needed):
                        
                                    [program:frappe-bench-web]
                                    command=/home/jeffchavezdev/frappe-bench/env/bin/gunicorn -b 0.0.0.0:8000 -w 2 frappe.app:application --preload
                                    directory=/home/jeffchavezdev/frappe-bench/sites
                                    user=jeffchavezdev
                                    autostart=true
                                    autorestart=true
                                    stopwaitsecs=60
                                    
                                    [program:frappe-bench-schedule]
                                    command=/usr/local/bin/bench schedule
                                    directory=/home/jeffchavezdev/frappe-bench
                                    user=jeffchavezdev
                                    autostart=true
                                    autorestart=true
                                    stopwaitsecs=60
                                    
                                    [program:frappe-bench-worker-1]
                                    command=/usr/local/bin/bench worker --queue default
                                    directory=/home/jeffchavezdev/frappe-bench
                                    user=jeffchavezdev
                                    autostart=true
                                    autorestart=true
                                    stopwaitsecs=60
                                    
                                    [program:frappe-bench-worker-2]
                                    command=/usr/local/bin/bench worker --queue short
                                    directory=/home/jeffchavezdev/frappe-bench
                                    user=jeffchavezdev
                                    autostart=true
                                    autorestart=true
                                    stopwaitsecs=60
                                    
                                    [program:frappe-bench-worker-3]
                                    command=/usr/local/bin/bench worker --queue long
                                    directory=/home/jeffchavezdev/frappe-bench
                                    user=jeffchavezdev
                                    autostart=true
                                    autorestart=true
                                    stopwaitsecs=60
                        Update and Start Supervisor: Reload Supervisor configuration and start the services:

                              sudo supervisorctl reread
                              sudo supervisorctl update
                              sudo supervisorctl start all
                        Verify Supervisor Services: Check the status of the services:
                              sudo supervisorctl status
      
      # Error 5: Bench start error 
            Stop the Gunicorn Processes - sudo kill -9 [14193 14202 14203 or specify processes that use port 8000]
            Change port 
                  nano Procfile
                  web: bench serve --port 8001
                  bench start
                                  
      # When updating the local instance's data 
            Download the database from the portal
            bench --site aos.erp.local restore manual-backups/database.sql
            bench --site aos.erp.local migrate

      # Set Admin Password
            bench --site site-name set-admin-password sample_password


      # NO SUCH GROUP ERROR:
            https://discuss.frappe.io/t/very-common-error/114076/16
                        

    
