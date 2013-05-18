# Server Configuration

### Get the essentials

$ sudo apt-get update

$ sudo apt-get upgrade

$ sudo apt-get install nano tcpdump strace

### Check for unwanted listeners


$ netstat -alpn

### Re-configure the timezone

$ date

$ more /etc/timezone

$ sudo dpkg-reconfigure tzdata

$ sudo service cron stop

$ sudo service cron start

### Add a new user and include on the sudoers list

$ sudo useradd -m -s /bin/bash [USERNAME]

$ sudo passwd [USERNAME]

$ sudo adduser [USERNAME] sudo

$ sudo service ssh reload
 
### Edit and uncomment the `ListenAddress` statement, alert the `PermitRootLogin` statement, and add the `AllowUsers` statement

$ sudo nano /etc/ssh/sshd_config

    ListenAddress 0.0.0.0
    PermitRootLogin no
    AllowUsers [USERNAME]

### Add the following lines under the file's `restrict` statements

$ sudo nano /etc/ntp.conf

    # Disable wild card listener
    interface ignore wildcard

$ sudo service ntp restart

# Gitlab 5.1.0 Installation 

Based on [https://github.com/gitlabhq/gitlabhq/blob/master/doc/install/installation.md](https://github.com/gitlabhq/gitlabhq/blob/master/doc/install/installation.md)

### Prepare the system. 
#### Do you want `postfix`? (Don't forget to add it if you do...)

$ sudo apt-get install -y build-essential nano zlib1g-dev libyaml-dev libssl-dev libgdbm-dev libreadline-dev libncurses5-dev libffi-dev curl git-core openssh-server redis-server checkinstall libxml2-dev libxslt-dev libcurl4-openssl-dev libicu-dev

$ sudo apt-get install -y python2.7 ruby1.9.1

$ sudo gem install bundler

### Install gitlab-shell

$ sudo adduser --disabled-login --gecos 'GitLab' git

$ sudo su git

$ cd /home/git

$ git clone https://github.com/gitlabhq/gitlab-shell.git

$ cd gitlab-shell

$ git checkout v1.3.0

$ cp config.yml.example config.yml

### Edit and replace the `gitlab_url` statement with the appropriate url

$ nano -S config.yml

    gitlab_url: "http://[FQDN]/"

### Install and change back to the original user account

$ ./bin/install

$ exit

### Install mysql

$ sudo apt-get install -y mysql-server mysql-client libmysqlclient-dev

$ sudo mysql -u root -p

    CREATE USER 'gitlab'@'localhost' IDENTIFIED BY '[MYSQLUSERPASSWD]';
    CREATE DATABASE IF NOT EXISTS `gitlabhq_production` DEFAULT CHARACTER SET `utf8` COLLATE `utf8_unicode_ci`;
    USE gitlabhq_production;
    GRANT SELECT, LOCK TABLES, INSERT, UPDATE, DELETE, CREATE, DROP, INDEX, ALTER ON `gitlabhq_production`.* TO 'gitlab'@'localhost';
    EXIT;

$ sudo -u git -H mysql -u gitlab -p -D gitlabhq_production

### Prepare the directory structure for GitLab

$ cd /home/git

$ sudo -u git -H git clone https://github.com/gitlabhq/gitlabhq.git gitlab

$ cd /home/git/gitlab

$ sudo -u git -H git checkout 5-1-stable

$ cd /home/git/gitlab

$ sudo -u git -H cp config/gitlab.yml.example config/gitlab.yml

#### Change `localhost` to your fully-qualified domain name, and disable gravatar if you want

$ sudo -u git -H nano -S config/gitlab.yml

    host: [FQDN]
    port: 80
    gravatar: enabled: false

$ sudo chown -R git log/

$ sudo chown -R git tmp/

$ sudo chmod -R u+rwX log/

$ sudo chmod -R u+rwX tmp/

$ sudo -u git -H mkdir /home/git/gitlab-satellites

$ sudo -u git -H mkdir tmp/pids/

$ sudo -u git -H mkdir tmp/sockets/

$ sudo chmod -R u+rwX tmp/pids/

$ sudo chmod -R u+rwX tmp/sockets/

$ sudo -u git -H mkdir public/uploads

$ sudo chmod -R u+rwX public/uploads

$ sudo -u git -H cp config/puma.rb.example config/puma.rb

$ sudo -u git -H git config --global user.name "GitLab"

$ sudo -u git -H git config --global user.email "`gitlab@localhost`"

$ sudo -u git cp config/database.yml.mysql config/database.yml

### Update the database configuration file

$ sudo -u git -H nano -S config/database.yml

    username: gitlab
    password: "[MYSQLUSERPASSWD]"

### Install GitLab

$ cd /home/git/gitlab

$ sudo apt-get install ruby1.9.1-dev

$ sudo gem install json --version '1.8.0'

$ sudo gem install charlock_holmes --version '0.6.9.4'

#### Be sure to set the system to use ruby1.9.1 and not ruby1.8 incase there are multiple installations

$ sudo update-alternatives --config ruby

#### Install GitLab `--without` support for ...

$ sudo -u git -H bundle install --deployment --without development test postgres

$ sudo -u git -H bundle exec rake gitlab:setup RAILS_ENV=production

### Startup script

$ sudo curl --output /etc/init.d/gitlab https://raw.github.com/gitlabhq/gitlabhq/master/lib/support/init.d/gitlab

$ sudo chmod +x /etc/init.d/gitlab

$ sudo update-rc.d gitlab defaults 21

### Run some tests

$ sudo -u git -H bundle exec rake gitlab:env:info RAILS_ENV=production

    System information
    System: Ubuntu 12.04
    Current User: git
    Using RVM: no
    Ruby Version: 1.9.3p0
    Gem Version: 1.8.11
    Bundler Version: 1.3.5
    Rake Version: 10.0.4
    
    GitLab information
    Version: 5.1.0
    Revision: a06d9a4
    Directory: /home/git/gitlab
    DB Adapter: mysql2
    URL: http://[FQDN]:80
    HTTP Clone URL: http://[FQDN]:80/some-project.git
    SSH Clone URL: git@[FQDN]:some-project.git
    Using LDAP: no
    Using Omniauth: no
    
    GitLab Shell
    Version: 1.3.0
    Repositories: /home/git/repositories/
    Hooks: /home/git/gitlab-shell/hooks/
    Git: /usr/bin/git

### A second test, it will notify you to start `sidekiq`, ignore the init script notification

$ sudo -u git -H bundle exec rake gitlab:check RAILS_ENV=production

    Try fixing it:
        sudo -u git -H bundle exec rake sidekiq:start RAILS_ENV=production
    Try fixing it:
        Redownload the init script
        doc/install/installation.md in section "Install Init Script"

### Start up the services, netstat should should a gitlab unix listen socket

$ sudo service gitlab start

$ netstat -alpn

### Install nginx

$ sudo apt-get install nginx

$ sudo curl --output /etc/nginx/sites-available/gitlab https://raw.github.com/gitlabhq/gitlabhq/master/lib/support/nginx/gitlab

$ sudo ln -s /etc/nginx/sites-available/gitlab /etc/nginx/sites-enabled/gitlab

#### Set nginx listener and fqdn

$ sudo nano /etc/nginx/sites-enabled/gitlab

    listen *:80 default_server;
    server_name [FQDN];

$ cd /etc/nginx/sites-enabled

$ sudo unlink default

$ sudo service nginx restart

#### Check you still have sensible listen sockets

$ netstat -alpn

### Login and change the default credentials

    url: http://[FQDN]:80/
    login: admin@local.host
    password: 5iveL!fe

### Restart the system to check it all comes back

$ sudo restart -r now

