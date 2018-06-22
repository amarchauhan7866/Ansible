Firstly i have created 2 machine one for ansible and one for node
make the ssh connection using ssh-keygen 

checked the connectivity from ansible server

[root@localhost ~]# ansible -m ping app
192.168.1.111 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}

###################################################################################

Add new key to authorized_keys files and removed the privious key (exclusive) using the authorized_keys modules

Now created the playbook for the same 


####################################################################################
---

  - hosts: app
    become_user: root
    tasks:

    - name: Setauthorized key took from file
      authorized_key:
        user: root
        state: present
        key: "{{ lookup('file', '/root/.ssh/id_rsa.pub') }}"
        exclusive: yes

![authorized](https://github.com/amarchauhan7866/Ansible/blob/master/Media/authorized.PNG)

######################################################################################

![AuthorizedKeyoutput](https://github.com/amarchauhan7866/Ansible/blob/master/Media/AuthorizedKeyoutput.PNG)

Results:-

[root@localhost ~]# ansible -m ping app
192.168.1.111 | SUCCESS => {
    "changed": false,
    "ping": "pong"
}
[root@localhost ~]#

~
