Assignment Details:
Jenkins Server: 192.168.1.110
Ansible Server: 192.168.1.101
Deployment_ Server: 192.168.1.111

Created the Ansible Server as a Slave in Jenkins server
![Slave](https://github.com/amarchauhan7866/Ansible/blob/master/Media/Slave.png)

#########################################################

Now created the Yml file
---
 - hosts: app
   become: yes
   become_user: root
   tasks:
    - name: Install a java pkg
      yum: name=java state=latest

    - name: Install Tomcat
      yum: name=tomcat state=latest
    - name: start a tomcat service
      service: name=tomcat state=restarted
    - name: Install tomcat admin pkg
      yum: name=tomcat-webapps state=latest
    - name: Install tocamcat doc
      yum: name=tomcat-docs-webapp state=latest
    - name: Install tomcat javadoc
      yum: name=tomcat-javadoc state=latest

    - name: tomcat service enable
      service: name=tomcat enabled=yes
    - name: Firewalld Stop service
      service: name=firewalld state=stopped


#############################################################
![Provisionyml](https://github.com/amarchauhan7866/Ansible/blob/master/Media/provisionyml.png)
![ jenkinsprovisionoutput](https://github.com/amarchauhan7866/Ansible/blob/master/Media/jenkinsprovisionoutput.png)






#########################################################################################

---
 - hosts: app
   become: yes
   become_user: root
   tasks:
     - name: Clone Git Repo
       git:
         repo: 'https://github.com/amarchauhan7866/ContinuousIntegration.git'
         dest: /buildrepo1
         clone: yes
         update: no
     - name: Installed maven
       yum: name=maven state=latest update_cache=yes
     - name: building package
       command: mvn package install
       args:
          chdir: /buildrepo1/Spring3HibernateApp/

![ buildyml](https://github.com/amarchauhan7866/Ansible/blob/master/Media/buildyml.png)

![ ansiblebuildoutput](https://github.com/amarchauhan7866/Ansible/blob/master/Media/ansiblebuildoutput.png)


##################################################################################

---
 - hosts: app
   become: yes
   tasks:
    - name: "copy war file"
      copy:
        src: /buildrepo1/Spring3HibernateApp/target/Spring3HibernateApp.war
        dest: /usr/share/tomcat/webapps/
        remote_src: yes

![deploymentyml](https://github.com/amarchauhan7866/Ansible/blob/master/Media/deploymentyml.png)

![ ansibledeploymentoutput](https://github.com/amarchauhan7866/Ansible/blob/master/Media/ansibledeploymentoutput.png)


##############################################################################################

Final Ouput


![ Deploymentfinaloutput](https://github.com/amarchauhan7866/Ansible/blob/master/Media/Deploymentfinaloutput.png)


