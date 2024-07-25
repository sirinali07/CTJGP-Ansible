## Lab 3: Implementing Ansible Playbook
```
mkdir ansible-labs && cd ansible-labs
```

### Task 1: Playbook to install apache web server
```
vi install-apache-pb.yml
```
Add the given content, by pressing "INSERT"
```
---
- name: This play will install apache web servers on all the hosts
  hosts: all
  become: yes
  tasks:
    - name: Task1 will install httpd using yum
      yum:
        name: httpd
        #local cache of the package information available from the repositories configured on the system
        update_cache: yes
        state: latest
    - name: Task2 will upload custom index.html into all hosts
      copy:
        src: /home/ec2-user/ansible-labs/index.html
        dest: /var/www/html
    - name: Task3 will setup attributes for file
      file:
        path: /var/www/html/index.html
        owner: apache
        group: apache
        mode:  0644
    - name: Task4 will start the httpd
      service:
        name: httpd
        state: started
```
**save the file using** `ESCAPE + :wq!`

State as 'Present' and 'Installed' are used interchangeably. They both do the same thing i.e. It 
will ensure that a desired package is installed. Whereas State as 'Latest' means in addition
to installation, it will go ahead and update if it is not of the latest available version.

Now lets create an index.html file to be used as the landing page for the web server.
In task2 of the above playbook, this 'index.html' will be copied to the document root of the 
httpd server.
```
vi index.html
```

Add the given content, by pressing "INSERT" 
```
<html>
  <body>
  <h1>Welcome to Ansible Training from CloudThat</h1>
  </body>
</html>
```
**save the file using** `ESCAPE + :wq!`

Now run the playbook so that it installs httpd to all the hosts and index.html is copied from 
the ansible-control
```
ansible-playbook install-apache-pb.yml
```
```
curl <private_ip of node1> 
```
```
curl <private_ip of node2>
```

---------------------------------------------------------------------------------------
### Task 2: Uninstall apache web server

With slight modification, we can change the playbook to uninstall apache (self exercise)
```
cp install-apache-pb.yml uninstall-apache-pb.yml
```
```
vi uninstall-apache-pb.yml
```
Retain only first task. Replace 'state: latest' with 'state: absent'

Check if the playbook is ok
```
ansible-playbook uninstall-apache-pb.yml --check
```

Now run the playbook and check in the browser if the web page exists.
```
ansible-playbook uninstall-apache-pb.yml
```

Check the browser with the corresponding IPv4 DNS name and ensure webpage is removed.
