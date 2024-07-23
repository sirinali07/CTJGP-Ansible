## Lab : Exploring Ad-Hoc Commands

```
sudo vi /etc/ansible/hosts
```

Add the given line, by pressing "INSERT" 
add localhost and add the connection as local so that it wont try to use ssh
```
localhost ansible_connection=local
```
**save the file using** `ESCAPE + :wq!`

In real life situations, one of the managed node may be used as the ansible control node.
In such cases, we can make it a managed node, by adding localhost in hosts inventory file.


get memory details of the hosts using the below ad-hoc command
```
ansible all -m command -a "free -h"
```
OR
```
ansible all -a "free -h"
```

Create a user ansible-new in the 2 nodes + the control node
This creates the new user and the home directory /home/ansible-new
```
ansible all -m user -a "name=ansible-new" --become
```

lists all users in the machine. Check if ansible-new is present in the managed nodes / localhost
```
ansible node1 -a "cat /etc/passwd"
```

List all directories in /home. Ensure that directory 'ansible-new' is present in /home. 
```
ansible node2 -a "ls /home"
```

Change the permission mode from '700' to '755' for the new home directory created for ansible-new
```
ansible node1 -m file -a "dest=/home/ansible-new mode=755" --become
```

Check if the permissions got changed
```
ansible node1 -a "sudo ls -l /home"
```

Create a new file in the new dir in node 1
```
ansible node1 -m file -a "dest=/home/ansible-new/demo.txt mode=600 state=touch" --become
```

Check if the permissions got changed
```
ansible node1 -a "sudo ls -l /home/ansible-new/"
```

Add content into the file
```
ansible node1 -b -m lineinfile -a 'dest=/home/ansible-new/demo.txt line="This server is managed by Ansible"'
```

check if the lines are added in demo.txt
```
ansible node1 -a "sudo cat /home/ansible-new/demo.txt"
```

You can remove the line using parameter state=absent
```
ansible node1 -b -m lineinfile -a 'dest=/home/ansible-new/demo.txt line="This server is managed by Ansible" state=absent'
```

check if the lines are removed from demo.txt
```
ansible node1 -b -a "sudo cat /home/ansible-new/demo.txt"
```

Now copy a file from ansible-control node to host node 1
```
touch test.txt
```
```
echo "This file will be copied to managed node using copy module" >> test.txt
```
```
ansible node1 -m copy -a "src=test.txt dest=/home/ansible-new/test" -b
```
--become can be replaced by -b

check if the file got copied to managed node.
```
ansible node1 -b -a "sudo ls -l /home/ansible-new/test"
```
```
sudo vi /etc/ansible/hosts
```

Remove the below line from hosts inventory file. 
localhost ansible_connection=local

**save the file using** `ESCAPE + :wq!`
