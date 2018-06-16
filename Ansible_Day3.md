1.  **Create and delete ninja directory on host machine**

**[root\@amar \~]\# ansible webservers -a "mkdir ninja"**

192.168.1.11 \| SUCCESS \| rc=0 \>\>

192.168.1.12 \| SUCCESS \| rc=0 \>\>

**[root\@amar \~]\# ansible webservers -a "rm -rf ninja"**

192.168.1.11 \| SUCCESS \| rc=0 \>\>

192.168.1.12 \| SUCCESS \| rc=0 \>\>

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

1.  **Set hostname on all nodes from remote machine**

**[root\@amar \~]\# ansible all -m command -a 'hostname rana'**

192.168.1.11 \| SUCCESS \| rc=0 \>\>

1.  SUCCESS \| rc=0 \>\>

>   **3.Fetch os of all nodes and store o/p into a file, use min two different
>   machine of different flavour of os.**

[root\@amar \~]\# **ansible all -m setup -a "filter=ansible_distribution\*" \>\>
/root/rana/os**

**[root\@amar \~]\# cat /root/rana/os**

**192.168.1.125 \| SUCCESS =\> {**

"ansible_facts": {

**"ansible_distribution": "Debian",**

"ansible_distribution_file_parsed": true,

"ansible_distribution_file_path": "/etc/os-release",

"ansible_distribution_file_variety": "Debian",

"ansible_distribution_major_version": "9",

"ansible_distribution_release": "stretch",

**"ansible_distribution_version": "9.4"**

},

"changed": false

}

**192.168.1.102 \| SUCCESS =\> {**

"ansible_facts": {

**"ansible_distribution": "CentOS",**

"ansible_distribution_file_parsed": true,

"ansible_distribution_file_path": "/etc/redhat-release",

"ansible_distribution_file_variety": "RedHat",

"ansible_distribution_major_version": "6",

"ansible_distribution_release": "Final",

**"ansible_distribution_version": "6.9"**

},

"changed": false

}

**192.168.1.11 \| SUCCESS =\> {**

"ansible_facts": {

**"ansible_distribution": "CentOS",**

"ansible_distribution_file_parsed": true,

"ansible_distribution_file_path": "/etc/redhat-release",

"ansible_distribution_file_variety": "RedHat",

"ansible_distribution_major_version": "6",

"ansible_distribution_release": "Final",

**"ansible_distribution_version": "6.9"**

},

"changed": false

\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

1.  **Add three group into hosts file through ansible module.**

    **[root\@amar \~]\# ansible localhost -m lineinfile -u root -a
    "path=/etc/ansible/hosts
    line=[Debian]\\n192.168.1.125\\n[Centos]\\n192.168.1.11\\n[All]\\n192.168.1.125\\n192.168.1.11\\n192.168.1.102"**

    localhost \| SUCCESS =\> {

    "backup": "",

    "changed": true,

    "msg": "line added"

    }

    **You have new mail in /var/spool/mail/root**

    **[root\@amar \~]\# cat /etc/ansible/hosts**

    [webservers]

    192.168.1.11

    192.168.1.102

    192.168.1.125 ansible_user=amar

    [app]

    [web]

    **[Debian]**

    **192.168.1.125**

    **[Centos]**

    **192.168.1.11**

    **[All]**

    **192.168.1.125**

    **192.168.1.11**

    **192.168.1.102**

    **[root\@amar \~]\#**

    **\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#**

2.  **Problem statement: Using Adhoc command**

    **\* Install apache on Debian machine**

    ansible debian -m apt -u root -a 'name=apache2 state=present'

    **Cross check apache installed or not from remote machine**

    ansible Debian -a ‘service apache2 status’

    192.168.1.102 \| SUCCESS \| rc=0 \>\>

    Apache2 is running (pid 2089).

    \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

    **Apache run on 8082 port**

    **[root\@amar \~]\# ansible debian -m lineinfile -u root -a
    "path=/etc/apache2/ports.conf regexp='\^(\\s+)Listen 80(\\s+)\$'
    line='listen 8082'"**

    192.168.1.102 \| SUCCESS =\> {

    "backup": "",

    "changed": true,

    "msg": "line added"

    }

    **Create two virtual host**

    **Restart apache from remote machine**

    **[root\@amar \~]\# ansible debian -a 'service apache2 restart'**

    192.168.1.102 \| SUCCESS \| rc=0 \>\>

    Restarting web server: apache2 ... waiting .

    \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#

    **Install nginx on centos machine**

    **[root\@amar \~]\# ansible Centos -a 'yum install epel-release'**

    192.168.1.11 \| SUCCESS \| rc=0 \>\>

    Loaded plugins: fastestmirror

    Setting up Install Process

    Loading mirror speeds from cached hostfile

    \* base: del-mirrors.extreme-ix.org

    \* epel: del-mirrors.extreme-ix.org

    \* extras: del-mirrors.extreme-ix.org

    \* updates: del-mirrors.extreme-ix.org

    Package epel-release-6-8.noarch already installed and latest version

    Nothing to
    dohttp://del-mirrors.extreme-ix.org/epel/6/x86_64/repodata/repomd.xml:
    [Errno -1] repomd.xml does not match metalink for epel

    Trying other mirror.

    **[root\@amar \~]\# ansible Centos -a 'yum install nginx -y'**

    192.168.1.11 \| SUCCESS \| rc=0 \>\>

    Loaded plugins: fastestmirror

    Setting up Install Process

    Loading mirror speeds from cached hostfile

    \* base: del-mirrors.extreme-ix.org

    \* epel: del-mirrors.extreme-ix.org

    \* extras: del-mirrors.extreme-ix.org

    \* updates: del-mirrors.extreme-ix.org

    Resolving Dependencies

    \--\> Running transaction check

    \---\> Package nginx.x86_64 0:1.10.2-1.el6 will be installed

    \--\> Processing Dependency: nginx-all-modules = 1.10.2-1.el6 for package:
    nginx-1.10.2-1.el6.x86_64

    \--\> Running transaction check

    \---\> Package nginx-all-modules.noarch 0:1.10.2-1.el6 will be installed

    \--\> Processing Dependency: nginx-mod-stream = 1.10.2-1.el6 for package:
    nginx-all-modules-1.10.2-1.el6.noarch

    \--\> Processing Dependency: nginx-mod-mail = 1.10.2-1.el6 for package:
    nginx-all-modules-1.10.2-1.el6.noarch

    \--\> Processing Dependency: nginx-mod-http-xslt-filter = 1.10.2-1.el6 for
    package: nginx-all-modules-1.10.2-1.el6.noarch

    \--\> Processing Dependency: nginx-mod-http-perl = 1.10.2-1.el6 for package:
    nginx-all-modules-1.10.2-1.el6.noarch

    \--\> Processing Dependency: nginx-mod-http-image-filter = 1.10.2-1.el6 for
    package: nginx-all-modules-1.10.2-1.el6.noarch

    \--\> Processing Dependency: nginx-mod-http-geoip = 1.10.2-1.el6 for
    package: nginx-all-modules-1.10.2-1.el6.noarch

    \--\> Running transaction check

    \---\> Package nginx-mod-http-geoip.x86_64 0:1.10.2-1.el6 will be installed

    \---\> Package nginx-mod-http-image-filter.x86_64 0:1.10.2-1.el6 will be
    installed

    \---\> Package nginx-mod-http-perl.x86_64 0:1.10.2-1.el6 will be installed

    \---\> Package nginx-mod-http-xslt-filter.x86_64 0:1.10.2-1.el6 will be
    installed

    \---\> Package nginx-mod-mail.x86_64 0:1.10.2-1.el6 will be installed

    \---\> Package nginx-mod-stream.x86_64 0:1.10.2-1.el6 will be installed

    \--\> Finished Dependency Resolution

    Dependencies Resolved

    ================================================================================

    Package Arch Version Repository

    Size

    ================================================================================

    Installing:

    nginx x86_64 1.10.2-1.el6 epel 462 k

    Installing for dependencies:

    nginx-all-modules noarch 1.10.2-1.el6 epel 7.7 k

    nginx-mod-http-geoip x86_64 1.10.2-1.el6 epel 14 k

    nginx-mod-http-image-filter x86_64 1.10.2-1.el6 epel 16 k

    nginx-mod-http-perl x86_64 1.10.2-1.el6 epel 26 k

    nginx-mod-http-xslt-filter x86_64 1.10.2-1.el6 epel 16 k

    nginx-mod-mail x86_64 1.10.2-1.el6 epel 43 k

    nginx-mod-stream x86_64 1.10.2-1.el6 epel 36 k

    Transaction Summary

    ================================================================================

    Install 8 Package(s)

    Total download size: 620 k

    Installed size: 1.6 M

    Downloading Packages:

    \--------------------------------------------------------------------------------

    Total 236 kB/s \| 620 kB 00:02

    Running rpm_check_debug

    Running Transaction Test

    Transaction Test Succeeded

    Running Transaction

    Installing : nginx-mod-http-geoip-1.10.2-1.el6.x86_64 1/8

    Installing : nginx-mod-stream-1.10.2-1.el6.x86_64 2/8

    Installing : nginx-mod-http-perl-1.10.2-1.el6.x86_64 3/8

    Installing : nginx-mod-http-image-filter-1.10.2-1.el6.x86_64 4/8

    Installing : nginx-mod-http-xslt-filter-1.10.2-1.el6.x86_64 5/8

    Installing : nginx-1.10.2-1.el6.x86_64 6/8

    Installing : nginx-mod-mail-1.10.2-1.el6.x86_64 7/8

    Installing : nginx-all-modules-1.10.2-1.el6.noarch 8/8

    Verifying : nginx-mod-mail-1.10.2-1.el6.x86_64 1/8

    Verifying : nginx-mod-http-geoip-1.10.2-1.el6.x86_64 2/8

    Verifying : nginx-mod-stream-1.10.2-1.el6.x86_64 3/8

    Verifying : nginx-mod-http-perl-1.10.2-1.el6.x86_64 4/8

    Verifying : nginx-all-modules-1.10.2-1.el6.noarch 5/8

    Verifying : nginx-mod-http-image-filter-1.10.2-1.el6.x86_64 6/8

    Verifying : nginx-1.10.2-1.el6.x86_64 7/8

    Verifying : nginx-mod-http-xslt-filter-1.10.2-1.el6.x86_64 8/8

    Installed:

    nginx.x86_64 0:1.10.2-1.el6

    Dependency Installed:

    nginx-all-modules.noarch 0:1.10.2-1.el6

    nginx-mod-http-geoip.x86_64 0:1.10.2-1.el6

    nginx-mod-http-image-filter.x86_64 0:1.10.2-1.el6

    nginx-mod-http-perl.x86_64 0:1.10.2-1.el6

    nginx-mod-http-xslt-filter.x86_64 0:1.10.2-1.el6

    nginx-mod-mail.x86_64 0:1.10.2-1.el6

    nginx-mod-stream.x86_64 0:1.10.2-1.el6

    Complete!

    **[root\@amar \~]\# ansible Centos -a 'service nginx restart'**

    192.168.1.11 \| SUCCESS \| rc=0 \>\>

    Stopping nginx: [FAILED]

    Starting nginx: [ OK ]

    **[root\@amar \~]\# ansible Centos -a 'service nginx status'**

    192.168.1.11 \| SUCCESS \| rc=0 \>\>

    nginx (pid 1704) is running...

    \#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#\#
