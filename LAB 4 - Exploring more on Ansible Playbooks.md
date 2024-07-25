## Lab : Exploring more on Ansible Playbooks
-----------------------------------------------------------------------------------
### Task 1: Creating a playbook and adding content to a file
```
cd /home/ec2-user/ansible-labs
```
```
vi putfile.yml
```

Add the given content, by pressing "INSERT" 
```
---
- hosts: all
  become: yes
  tasks:
    - name: Creating a new user cloudthat
      user:
        name: cloudthat
    - name: Creating a directory for the new user
      file:
        path: /home/cloudthat
        state: directory
```


**save the file using** `ESCAPE + :wq!`

Now run the playbook as follow
```
ansible-playbook putfile.yml
```
Now modify the playbook to add another child directory and a file inside cloudthat's home directory
```
vi putfile.yml
```
Add the given content, by pressing "INSERT" 
```
---
- hosts: all
  become: yes
  tasks:
    -   name: Creating a new user cloudthat
        user:
          name: cloudthat
    -   name: Creating a directory for the new user
        file:
          path: /home/cloudthat
          state: directory
    -   name: creating a folder named ansible
        file:
          path: /home/cloudthat/ansible
          state: directory
    -   name: creating a file within the folder ansible
        file:
          path: /home/cloudthat/ansible/hello.txt
          state: touch
    -   name: Changing owner and group with permission for the file within the folder named ansible
        file:
          path: /home/cloudthat/ansible/hello.txt
          owner: root
          group: cloudthat
          mode: 0665
    -   name: adding a block of string to the file created named hello.txt
        blockinfile:
          path: /home/cloudthat/ansible/hello.txt
          block: |
            This is line 1
            This is line 2

```
**save the file using** `ESCAPE + :wq!`
if you use '|' then the new line characters will be retained.

we can execute it step by step
```
ansible-playbook putfile.yml --step
```

check if the lines are added in hello.txt
```
ansible all -a "sudo cat /home/cloudthat/ansible/hello.txt"
```

Check if the permissions / ownership is as per the playbook
```
ansible all -a "sudo ls -l /home/cloudthat/ansible/"
