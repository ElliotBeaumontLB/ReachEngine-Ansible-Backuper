# ReachEngine-Ansible-Backuper
Backup core ReachEngine files with Ansible

NOTE: The list of files to backup intentionally includes files that might not exist, in order to capture all possible scenarios.  Thus there is an ignore flag set when backing up files, so all the other valid files will be backed up.  It is recommended to double check the destination output to ensure all files that you intend to capture have been properly backed up.

## How to use
1. Install Ansible and dependencies locally, and clone this repo.  We recommend running this from the Bastion host.
2. Copy a hosts.yml file from another Ansible inventory, or configure a hosts.yml file with your services and host IP addresses.  A hosts.example.yml file is provided for your convenience. 
3. Configure your "extra_vars.yml" file.  Default values for each variable are provided.  
3. Configure the ansible.cfg file with your SSH Key file location, so ansible can SSH from bastion host to ec2 instances. 
3. Run the "runme.sh" scrip, or choose one of the following commands to run. 

### Commands
For All hosts:\
`ansible-playbook -i hosts.yml -e @extra_vars.yml backup-playbook.yml`\
\
For a specific service:\
`ansible-playbook -i hosts.yml -e @extra_vars.yml -l nginx backup-playbook.yml`\
\
To override a variable when running ansible:\
`ansible-playbook -i hosts.yml -e @extra_vars.yml -e "backup_to_local=false" backup-playbook.yml`\
\

## Default Values
#### backup_to_local
Default: `backup_to_local: true`\
Setting `backup_to_local` to true means the files are backed up on the device running the ansible-playbook command (recommended Bastion).\
Setting this to false will copy the files to back up into the configured directory on the remote hosts.\

#### backup_directory
Default: `backup_directory: "/home/reachengine/"`\
This sets the backup destination directory for both local and remote locations.\
There should be a trailing `/` slash in this variable's value.\
