1. Fetch and display to STDOUT Ansible facts using the setup module.

[root\@amar \~]\# ansible all -m setup --tree /tmp/facts

192.168.1.11 \| SUCCESS =\> {

"ansible_facts": {

"ansible_all_ipv4_addresses": [

"192.168.1.11"

],

"ansible_all_ipv6_addresses": [

"fe80::a00:27ff:fe6b:b72"

],

"ansible_apparmor": {

"status": "disabled"

},

"ansible_architecture": "x86_64",

"ansible_bios_date": "12/01/2006",

"ansible_bios_version": "VirtualBox",

"ansible_cmdline": {

2.Fetch and display only the "virtual" subset of facts for each host.

**[root\@amar \~]\# ansible all -m setup -a 'gather_subset=virtual'**

3. Fetch and display the value of fully qualified domain name (FQDN) of each
host from their Ansible facts

**[root\@amar \~]\# ansible all -m setup --tree /tmp/facts \| grep fqdn**

"ansible_fqdn": "localhost.localdomain",

"ansible_fqdn": "cm.ansibleopstree.com",

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

4 .Display the uptime of all hosts using the command module.

[root\@amar \~]\# ansible all -m command -a 'uptime'

192.168.1.12 \| SUCCESS \| rc=0 \>\>

12:16:05 up 29 min, 3 users, load average: 0.00, 0.00, 0.00

192.168.1.11 \| SUCCESS \| rc=0 \>\>

12:16:04 up 29 min, 3 users, load average: 0.00, 0.00, 0.00

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

5. Ping all hosts except the 'control' host using the --limit option

[root\@amar \~]\# ansible all -m ping --limit 'webservers:!192.168.1.12'

192.168.1.11 \| SUCCESS =\> {

"changed": false,

"ping": "pong"

}

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

6 .Setup Java8 on the hosts in the "App" group using the apt module.

[root\@amar \~]\# ansible webservers -m yum -a "name=java-1.8.0 state=present"

Command output is very long so i have run again this command:

[root\@amar \~]\# ansible webservers -m yum -a "name=java-1.8.0 state=present"

192.168.1.12 \| SUCCESS =\> {

"changed": false,

"msg": "",

"rc": 0,

"results": [

"1:java-1.8.0-openjdk-1.8.0.171-8.b10.el6_9.x86_64 providing java-1.8.0 is
already installed"

]

}

192.168.1.11 \| SUCCESS =\> {

"changed": false,

"msg": "",

"rc": 0,

"results": [

"1:java-1.8.0-openjdk-1.8.0.171-8.b10.el6_9.x86_64 providing java-1.8.0 is
already installed"

]

}

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

7. Create inventory groups app and web

Created both the groups in inventory file

8. set a cron on ansible control machine that will run every 1 minute , and
perform below tasks:-

[root\@amar \~]\# ansible webservers -m command -a "touch /var/log/ninja_name"

192.168.1.11 \| SUCCESS \| rc=0 \>\>

192.168.1.12 \| SUCCESS \| rc=0 \>\>

i m still working on it
