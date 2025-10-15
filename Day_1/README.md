# RH294: Red Hat Enterprise Linux Automation with Ansible Labs

This repository contains all labs completed for the RH294 course. It includes exercises from initial Ansible setup, automated web server configuration, structured role-based configuration, and securing sensitive data with Ansible Vault. All steps, playbooks, and roles used during the labs are included.

---

## Labs Overview and Steps Completed

**Lab 1: Initial Ansible Configuration and Ad-Hoc Execution**

* Installed Ansible Automation Platform on the control node.
* Generated SSH key on the control node: `ssh-keygen -t rsa -b 2048`
* Transferred the public key to the managed node: `ssh-copy-id client@192.168.142.129`
* Created inventory file `inventory` for the managed node:

  ```
  [managed]
  192.168.142.129 ansible_user=client
  ```
* Verified connectivity and functionality with ad-hoc command:

  ```bash
  ansible managed -i inventory -m shell -a "df -h"
  ```

**Lab 2: Automated Web Server Configuration Using Ansible Playbooks**

* Created `webserver.yml` to automate:

  * Nginx installation.
  * Customization of web page content.
* Ran the playbook:

  ```bash
  ansible-playbook -i inventory webserver.yml
  ```
* Verified the web server on the managed node using browser or `curl`.

**Lab 3: Structured Configuration Management with Ansible Roles**

* Created roles: `docker`, `kubectl`, `jenkins` with tasks, handlers, defaults, vars, and meta.
* Created `web_tools.yml` playbook to call the roles.
* Ran the playbook:

  ```bash
  ansible-playbook -i inventory web_tools.yml
  ```
* Verified installations:

  * Docker: `docker --version`
  * Kubectl: `kubectl version --client`
  * Jenkins: `systemctl status jenkins`

**Lab 4: Securing Sensitive Data with Ansible Vault**

* Created `vault.yml` with database credentials:

  ```yaml
  db_user: ivolve_user
  db_password: supersecret123
  db_name: iVolve
  ```
* Encrypted vault file: `ansible-vault create vault.yml`
* Created `mysql_setup.yml` playbook to:

  * Install MySQL server and client.
  * Ensure MySQL service is running.
  * Create database and user with privileges.
  * Validate database connection using encrypted variables.
* Ran the playbook with vault password:

  ```bash
  ansible-playbook -i inventory mysql_setup.yml --ask-vault-pass
  ```
* Verified MySQL manually on the managed node:

  ```bash
  ansible managed -i inventory -m shell -a "mysql -u ivolve_user -p'supersecret123' -e 'SHOW DATABASES;'"
  ```

---

## Repository Structure

```
ansible/
├── inventory
├── mysql_setup.yml
├── vault.yml
├── web_tools.yml
├── webserver.yml
└── roles/
    ├── docker/
    ├── jenkins/
    └── kubectl/
```

---

## Usage

1. Clone the repository:

```bash
git clone <repository_url>
```

2. Navigate to the ansible directory:

```bash
cd ansible
```

3. Run any playbook:

```bash
ansible-playbook -i inventory <playbook_name.yml> --ask-vault-pass
```

> Use `--ask-vault-pass` only for playbooks that include encrypted variables.

---

## Notes

* Ensure managed nodes are reachable via SSH.
* Provide correct vault password for encrypted variables.
* Verify services after running playbooks to confirm successful execution.
