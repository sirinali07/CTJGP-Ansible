## Lab : Implementing Ansible Roles

```
cd ~/
```
Lets uninstall httpd. After that, we will use ansible role to install it.
```
ansible-playbook /home/ec2-user/ansible-labs/uninstall-apache-pb.yml (Please provide the correct path name)
```

Install tree. A tree is a recursive directory listing program that produces a depth-indented listing of files. 
```
sudo yum install tree -y
```

You can view your home directory structure in tree format with below command tree 
```
tree /home/ec2-user/ansible-labs
```
Lets create the code for Role labs
```
cd ~/
```
```
mkdir roles && cd roles
```

Now inside the roles directory, create a different directory for roles, namely webrole. 
Now change your directory to webrole 
```
mkdir webrole && cd webrole/
```
```
mkdir files tasks && cd files/
```
```
vi index.html
```
```
<html>
  <body>
  <h1>We are performing the Roles Lab</h1>
  <img src= "https://d3ffutjd2e35ce.cloudfront.net/assets/logo1.png">
  </body>
</html>
```
**save the file using** `ESCAPE + :wq!`

Then go to the task directory as below and create main.yml 
```
cd .. && cd tasks/
```
```
vi main.yml
```
Add the given content, by pressing "INSERT" 
```
---
- name: install httpd
  yum: 
    name: httpd 
    update_cache: yes 
    state: latest

- name: uploading default index.html for host
  copy:
     src: /home/ec2-user/roles/webrole/files/index.html
     dest: /var/www/html

- name: Setting up attributes for file
  file:
    path:  /var/www/html/index.html
    owner: apache
    group: apache
    mode:  0644

- name: start httpd
  service:
    name=httpd 
    state=started
```
**save the file using** `ESCAPE + :wq!`

After the creation of this file, we are done with the complete hierarchy of roles, so we will have a look at how it is exactly using tree command
```
cd ../..
```
Now create directory and write codes as mention below
```
mkdir dbrole && cd dbrole
```
```
mkdir tasks && cd tasks
```
```
vi main.yaml
```
```
---
- name: install mariadb-server
  yum:
    name: mariadb-server
    update_cache: yes
    state: latest
- name: start mariadb
  service:
    name=mariadb
    state=started

```
```
cd ../..
```
```
tree
```

Now change the directory to ansible directory and create the playbook as implement-roles.yml
```
vi implement-roles.yml
```

Add the given content, by pressing "INSERT".
```
---
 - hosts: all
   become: yes 

   roles:
     - webrole
     - dbrole
```   
**save the file using** `ESCAPE + :wq!`

Execute the playbook
```
ansible-playbook implement_roles.yml
```
Check the home page on browser. (Public DNS)
It will show the webpage with msg "We are performing the Roles Lab"
