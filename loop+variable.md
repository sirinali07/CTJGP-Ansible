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
| `README.md` | Instructions and explanation of the lab. |

## 🛠️ How It Works

- The playbook reads a list of users (`user_list`) from `variable.yaml`.
- It **loops** through each user and creates them on the target machines.
- Each user is assigned a specific `uid`.

## 📜 Playbook Breakdown

- **hosts: all** — Target all machines listed in the inventory.
- **become: yes** — Run tasks with `sudo` privileges.
- **vars_files** — Import variables from `variable.yaml`.
- **Tasks** — Create users using a loop.

## 🚀 How to Run

1. Clone the repository:

    ```bash
    git clone <your-repo-url>
    cd ansible-user-creation-lab
    ```

2. Run the playbook:

    ```bash
    ansible-playbook -i inventory playbook.yaml
    ```

    (Replace `inventory` with your actual inventory file.)

## 📢 Notes
- Make sure the `inventory` file is correctly pointing to your target hosts.
- Ensure you have `become` privileges (sudo access) on the target machines.

---

Made with ❤️ using Ansible.
