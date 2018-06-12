• Use pip to install the ansible package and its dependencies to your control
machine

[root\@cm \~]\# yum install epel-release

[root\@cm \~]\# yum install python-pip\*

[root\@cm \~]\# pip install ansible

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

• Display the Ansible version and man page to STDOUT.

[root\@amar \~]\# which ansible

/usr/bin/ansible

[root\@amar \~]\# /usr/bin/ansible --version

ansible 2.5.4

config file = /root/.ansible.cfg

configured module search path = [u'/root/.ansible/plugins/modules',
u'/usr/share/ansible/plugins/modules']

ansible python module location = /usr/lib/python2.7/site-packages/ansible

executable location = /usr/bin/ansible

python version = 2.7.5 (default, Apr 11 2018, 07:36:10) [GCC 4.8.5 20150623 (Red
Hat 4.8.5-28)]

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

• Check all the possible parameters you need to know in ansible.cfg file

retry_files_save_path = \~/.ansible-retry

library = /usr/share/my_modules/

remote_port = 22

inventory file path = /etc/ansible/hosts

Forks = 5 default

Roles path =/etc/ansible/roles

vault_password_file = /path/to/vault_password_file

Host key checking = false

Remote user =root

Log path = /var/log/ansible.log

Plugins path =/usr/share/ansible/plugins/\*

• Ansible Inventory: Check the default inventory file for ansible control master
and use inventory, run ansible ping commands on various inventory groups. Try
this on minimum of two virtual machines.(You can use any of these
Docker/Vagrant)

Created the director and file and make the changes as per assignment

[root\@amar \~]\# cd .ansible

[root\@amar .ansible]\# mkdir retry-files

[root\@amar .ansible]\# cd ..

[root\@amar \~]\# vim .ansible.cfg

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

Now make the generate the ssh-keygen

[root\@amar \~]\# ssh-keygen

Generating public/private rsa key pair.

Enter file in which to save the key (/root/.ssh/id_rsa):

/root/.ssh/id_rsa already exists.

Overwrite (y/n)?

Copied generated file & checked it

[root\@amar \~]\# ssh-copy-id root\@192.168.1.11

[root\@amar \~]\# ssh root\@192.168.1.11

Last login: Wed Jun 13 02:03:50 2018 from amar.com

[root\@cm \~]\#

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

[root\@amar \~]\# ssh-copy-id root\@192.168.1.12

[root\@amar \~]\# ssh root\@192.168.1.12

Last login: Wed Jun 13 01:59:55 2018 from 192.168.1.100

[root\@localhost \~]\#

Now Setup the inventory hosts file and make the group

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

Test the connectivity form the cm to client

[root\@amar \~]\# ansible all -m ping

192.168.1.11 \| SUCCESS =\> {

"changed": false,

"ping": "pong"

}

192.168.1.12 \| SUCCESS =\> {

"changed": false,

"ping": "pong"

}

https://github.com/amarchauhan7866/Ansible/blob/master/Checkconnetivity.png

[root\@amar \~]\#

Now install the Nginx Using adhoc command

[root\@amar \~]\# ansible webservers -a "yum install nginx -y"

./media/image2.png

./media/image3.png

[root\@amar \~]\# ansible webservers -a "service nginx restart"

need to use command because service is insufficient you can add warn=False to
this

command task or set command_warnings=False in ansible.cfg to get rid of this
message.

https://github.com/amarchauhan7866/Ansible/blob/master/c511676b956ddacb7a8bb827d8bad34c.png

192.168.1.11 \| SUCCESS \| rc=0 \>\>

Stopping nginx: [ OK ]

Starting nginx: [ OK ]

192.168.1.12 \| SUCCESS \| rc=0 \>\>

Stopping nginx: [ OK ]

Starting nginx: [ OK ]

Fixed the error using ansible.cfg an set the parameter set
command_warnings=False

Now output

https://github.com/amarchauhan7866/Ansible/blob/master/Restartnginx.png

https://github.com/amarchauhan7866/Ansible/blob/master/adhocommand.png
