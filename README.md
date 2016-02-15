# Continuous Integration

## Installation
Install git on your server

sudo yum install git

## Server Configuration

1. Create user for apache for pulling from remote git

switch to root:
"sudo su"

create ssh dir for apache user:
"mkdir /var/www/.ssh"

setting the right permission:
"chown root -R /var/www/.ssh"

"chmod 777 /var/www/.ssh"

create key for apache:
sudo -u apache ssh-keygen -t rsa

leave default dir for id_rsa
leave empty for passphrase

change permission to 400 for apache key
chmod 400 /var/www/.ssh/id_rsa


## Git Configuration

### Step 1

clone git repo to current directory:
ssh-agent bash -c 'ssh-add /var/www/.ssh/id_rsa; git clone git@bitbucket.org:name/repo.git .'

give ownership of folder to apache so it can execute the php file from web load
"chown -R apache:apache /var/www/html/dev"

copy pull.php to another directory in your server /var/www/html/script/ (dont put in same directory as the git dir)

create a config file for git to /var/www/.ssh/config:
config

with this content:
Host bitbucket.org 
 User apache
 HostName bitbucket.org 
 IdentityFile /var/www/.ssh/id_rsa

add bitbucket.org to known host:
ssh-keyscan -t rsa bitbucket.org >> /var/www/.ssh/known_hosts

add the url to the pull.php file to your bitbucket webhook:
http://domain.com/script/pull.php?c=sg

manual terminal commands:
ssh-agent bash -c 'ssh-add /var/www/.ssh/id_rsa; git pull'

### Step 2

Disable listing of git files and directory, place in .htaccess:

  # disable directory listing
  Options -Indexes
  
  # disable git listing
  RedirectMatch 404 /\.git


