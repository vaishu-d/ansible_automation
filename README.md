# Ansible Playbooks

This repository contains various Ansible playbooks for automating tasks such as backing up configuration files, cleaning up old files, provisioning EC2 instances, creating users/groups, and installing Apache.

## 1. Backup Configuration Files
This playbook backs up configuration files by creating a unique directory under `/tmp/testbackup/` with the current timestamp for each backup run.

**Modules Used**:
- `ansible.builtin.file`
- `ansible.builtin.debug`
- `ansible.builtin.copy`

### Steps:
1. Create a timestamped directory for the backup.
2. Loop through the list of configuration files.
3. Output a message indicating the file being backed up.
4. Copy each file to the backup directory.

---

## 2. Cleanup Old Files and Generate HTML Report
This playbook searches for directories that match the pattern `config*`, finds files older than 1 day, deletes them, and generates an HTML report listing the deleted files.

**Modules Used**:
- `ansible.builtin.find`
- `set_fact`
- `ansible.builtin.file`
- `ansible.builtin.debug`
- `ansible.builtin.copy`

### Steps:
1. Search for directories matching `config*`.
2. Look inside the found directories for files older than 1 day..
3. Collect file paths and delete them.
4. Format deleted file information into an HTML report.

---

## 3. Create EC2 Instance
This playbook provisions multiple EC2 instances in a specified region using the Amazon AWS collection. Instance details like AMI ID and instance name are dynamically provided through a loop. Ansible Vault is used to securely store secret keys and access keys.

**Modules Used**:
- `amazon.aws.ec2_instance`
- `loop` (for iterating over instance details)

### Steps:
1. Provision EC2 instances with dynamic AMI IDs and instance names.
2. Fetch secret keys and access keys from Ansible Vault.
3. Run the playbook locally on the control node(localhost).

---

## 4. Create User & Group
This playbook creates a group called `testgroup` if it does not already exist and creates a user called `testuser`. It also verifies the user's login by running the `whoami` command with escalated privileges.

**Modules Used**:
- `ansible.builtin.group`
- `ansible.builtin.user`
- `ansible.builtin.command`
- `ansible.builtin.debug`

### Steps:
1. Create the `testgroup` if it doesn't exist.
2. Check for `testuser` and create it if needed.
3. Afterward, it verifies the login of the newly created testuser by running the whoami command with escalated privileges, switching to the testuser account.
4. The output of the whoami command is displayed using the debug module, showing the testuser username to confirm the login was successful.
   

---

## 5. Install Apache
This playbook installs Apache HTTPD (apache2), ensures it's up-to-date, and checks if Apache is configured to listen on port 80. If the configuration is correct, it starts the Apache service.

**Modules Used**:
- `apt`
- `command`
- `service`
- `debug`

### Steps:
1. Install and update Apache HTTPD (apache2).
2. Verify that Apache is listening on port 80 by checking `/etc/apache2/ports.conf`.
3. Start the Apache service.

---
