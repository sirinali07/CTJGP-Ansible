# 🚀 Ansible Lab: User Creation with Loop and External Variables

## 📝 Overview

This lab demonstrates how to create multiple Linux users using Ansible with:
- A **loop** in the playbook.
- **External variables** from a YAML file.

## 📂 Files Included

| File | Description |
|:-----|:------------|
| `playbook.yaml` | The main Ansible playbook to create users. |
| `variable.yaml` | External variables file containing the list of users. |


## 🛠️ How It Works

- The playbook reads a list of users (`user_list`) from `variable.yaml`.
- It **loops** through each user and creates them on the target machines.
- Each user is assigned a specific `uid`.

## 📜 Playbook Breakdown

- **hosts: all** — Target all machines listed in the inventory.
- **become: yes** — Run tasks with `sudo` privileges.
- **vars_files** — Import variables from `variable.yaml`.
- **Tasks** — Create users using a loop.
  
ansible-user-creation-lab/
├── playbook.yaml
├── variable.yaml
└── README.md

## 🚀 How to Run

Create a `playbook.yaml`
```
vi playbook.yaml
```
Add the given content, by pressing "INSERT"
```yaml
- name: User Creation Playbook
  hosts: all
  become: yes
  vars_files:
    - variable.yaml

  tasks:
    - name: Create users from the list
      user:
        name: "{{ item.name }}"
        uid: "{{ item.uid }}"
        state: present
      loop: "{{ user_list }}"
```
**save the file using** `ESCAPE + :wq!`

Create a `user_list.yaml` variable file

```
vi user_list.yaml
```
Add the given content, by pressing "INSERT"
```yaml
user_list:
  - { name: 'u1', uid: '8010' }
  - { name: 'u2', uid: '8020' }
  - { name: 'u3', uid: '8030' }
  - { name: 'u4', uid: '8040' }
```
**save the file using** `ESCAPE + :wq!`
Now run the playbook
```
ansible-playbook playbook.yaml
```
## 📢 Notes
- Make sure the `inventory` file is correctly pointing to your target hosts.
- Ensure you have `become` privileges (sudo access) on the target machines.

---


