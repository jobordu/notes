# Restoring GitLab from backup
Source: https://github.com/gitlabhq/gitlabhq/blob/4-2-stable/doc/raketasks/backup_restore.md

# Place your restore file here, in the format: /home/git/gitlab/tmp/backups/`[timestamp_of_backup]_gitlab_backup.tar`

$ cd /home/git/gitlab

# When only one restore file exists in the directory

$ sudo -u git -H bundle exec rake gitlab:backup:restore RAILS_ENV=production

# Or, to select a specific restore file, pass it as `[timestamp_of_backup]`

$ sudo -u git -H bundle exec rake gitlab:backup:restore RAILS_ENV=production BACKUP=`1368889053`
