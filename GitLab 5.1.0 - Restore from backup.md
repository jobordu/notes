# Restoring GitLab from backup
Source: https://github.com/gitlabhq/gitlabhq/blob/4-2-stable/doc/raketasks/backup_restore.md

#### Check your restore files are in the right place

And named correctly: `/home/git/gitlab/tmp/backups/[timestamp_of_backup]_gitlab_backup.tar`

$ cd /home/git/gitlab

#### When only one restore file exists in the directory

$ sudo -u git -H bundle exec rake gitlab:backup:restore RAILS_ENV=production

#### Or, to select a specific restore file, pass it as `[timestamp_of_backup]`

$ sudo -u git -H bundle exec rake gitlab:backup:restore RAILS_ENV=production BACKUP=`1368889053`

#### Troubleshooting
If you see a message like "Your current HEAD differs from the HEAD in the backup!" it means the verion of GitLab used to make this backup is not the same as the version you are currently running. You will be told the correct version to checkout from the source, so go back and re-install the correct version.

$ sudo -u git -H git checkout -b 5-1-stable a06d9a4992d3d3523a668f6923fe6ee8ebf23fac


