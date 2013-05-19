# Configuring a local backup job for GitLab using Cron

#### Install and Configure Postfix

$ sudo apt-get install postfix
	
$ sudo dpkg-reconfigure postfix

#### Configure Cron

$ sudo touch /home/git/cron.daily

$ sudo touch /home/git/cron.hourly

$ sudo nano /home/git/cron.weekly

    #!/bin/bash
    cd /home/git/gitlab
    /usr/local/bin/bundle exec rake gitlab:backup:create RAILS_ENV=production
    echo 
    echo Contents of /home/git/gitlab/tmp/backups/
    ls -l /home/git/gitlab/tmp/backups/
    echo 
    echo Finished local backup

$ sudo chmod 755 cron.hourly

$ sudo chmod 755 cron.daily

$ sudo chmod 755 cron.weekly

$ sudo crontab -u git -e

    # * * * * * bash /home/git/cron.hourly
    # * * * * * bash /home/git/cron.daily
    0 0 * * 0 bash /home/git/cron.weekly

