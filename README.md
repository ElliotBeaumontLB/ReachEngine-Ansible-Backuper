# ReachEngine-Ansible-Backuper
Backup core ReachEngine files with Ansible

NOTE: The list of files to backup intentionally includes files that might not exist, in order to capture all possible scenarios.  Thus there is an ignore flag set when backing up files, so all the other valid files will be backed up.  It is recommended to double check the destination output to ensure all files that you intend to capture have been properly backed up.

## How to use
1. Install Ansible and dependencies locally, and clone this repo.
2. Configure a "hosts.yml" file with your services and host IP addresses
3. Run one of the following commands to kick this off

### Commands
For All hosts:
`ansible-playbook -i hosts.yml backup-playbook.yml`

For a specific service:
`ansible-playbook -i hosts.yml -l nginx backup-playbook.yml`

To backup files on the remote host (not the local host)
`ansible-playbook -i hosts.yml -e "backup_to_local=false" backup-playbook.yml`

To change the destination backup directory (on the local host):
`ansible-playbook -i hosts.yml -e "backup_directory=/some/other/directory" backup-playbook.yml`

To backup files on the remote host, in a custom directory:
Option 1: multiple "-e" extra variables parameters:
`ansible-playbook -i hosts.yml -e "backup_to_local=false" -e "backup_directory=/some/other/directory" backup-playbook.yml`
Option 2: Space separated extra variables
`ansible-playbook -i hosts.yml -e "backup_to_local=false backup_directory=/some/other/directory" backup-playbook.yml`


## Default Values

`backup_to_local: true`
Setting `backup_to_local` to true means the files are backed up on the device running the ansible-playbook command.
Setting this to false will, the files will remain on the remote host (ie the remote nginx host).

`backup_directory: "/home/reachengine/"`
This sets the backup destination directory for both local and remote locations.  There should be a trailing `/` slash in this variable's value. 
If you run this playbook with `backup_to_local` set to true, it is recommended to modify the `backup_directory` since you likely do not have "/home/reachengine/" available on your local ansible machine.  
When running this playbook with `backup_to_local` set to false, then you may likely keep the `backup_directory` set to the default, as the reachengine user's home directory is likely already created on the remote hosts.

