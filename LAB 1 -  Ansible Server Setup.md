Login to AWS Console

# Lab : Installation and Configuration of Ansible

* Launch a RHEL 9 instance in us-east-1. 
* Choose t2.micro. 
* In security group, allow SSH (22), HTTP (80), and HTTPS (443) for all incoming traffic. 
* Add Tag Name: Ansible-ControlNode

### Once the EC2 is up & running, SSH into one of it and set the hostname as 'Control-Node'. 
```
sudo hostnamectl set-hostname Control-Node
```
```
bash
```

Update the package repository with latest available versions
```
sudo yum check-update
```

Install latest version of Python. 
```
sudo yum install python3-pip -y 
```
```
python3 --version
```
```
sudo pip3 install --upgrade pip
```

Install awscli, boto, boto3 and ansible
Boto/Boto3 are AWS SDK which will be needed while accessing AWS APIs
```
sudo pip3 install awscli boto boto3
```
```
sudo pip3 install ansible
```
```
pip show ansible
```

Authorize aws credentials
```
aws configure
```
#### Enter the Credentials as below. Example:
| **Access Key ID**    | **Secret Access Key** |
| -----------------    | --------------------- |
| 77AKIAWJXSSHRD27T6SC | 777H4Vh0U5oenKfmJ/+FEnGAmZvQLX7zTT |

Install wget so that we can download playbooks from the training material repository 
```
sudo yum install wget -y
```

Download the script using wget
```
wget https://devops-ansible-script-bk.s3.us-west-2.amazonaws.com/ansible_script.yaml
```

Execute the script
```
ansible-playbook ansible_script.yaml
```

Once you get the ip addresses, do the following:

```
sudo vi /etc/ansible/hosts
```

Add the prive IP addresses, by pressing "INSERT" 
```
  node1   ansible_ssh_host=<node1-private-ip>  ansible_ssh_user=ec2-user  
 
  node2   ansible_ssh_host=<node2-private-ip>  ansible_ssh_user=ec2-user  
```
e.g. node1 ansible_ssh_host=172.31.14.113 ansible_ssh_user=ec2-user
     node2 ansible_ssh_host=172.31.2.229 ansible_ssh_user=ec2-user


Save the file using "ESCAPE + :wq!"

list all managed node ip addresses.
```
ansible all --list-hosts
```

### SSH into each of them and set the hostnames.
```
ssh ec2-user@< Replace Node 1 IP >
```
```
sudo hostnamectl set-hostname managed-node-1
```
```
exit
```
```
ssh ec2-user@< Replace Node 2 IP >
```
```
sudo hostnamectl set-hostname managed-node-2
```
```
exit
```

### Use ping module to check if the managed nodes are able to interpret the ansible modules
```
ansible all -m ping
```
