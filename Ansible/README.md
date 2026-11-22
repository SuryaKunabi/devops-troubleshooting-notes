
# Ansible Troubleshooting – Real Issues & Solutions

This document contains troubleshooting notes from Ansible automation practice using an Ubuntu control environment and target server.

## 1. Ansible – SSH Connectivity to Target Server

**Problem:**

Ansible controller needs SSH connectivity to the managed Ubuntu target.

**Troubleshooting Commands:**

```bash
ansible all -i inventory -m ping
ssh ubuntu@<target-ip>
```

**Checks:**

- Verify target IP address.
- Verify SSH service status.
- Check SSH key configuration.
- Verify inventory username.
- Check security group or firewall rules.

**Learning:** Ansible uses SSH to communicate with Linux managed nodes.

---

## 2. Ansible – Privilege Escalation (Become)

**Problem:**

Ansible tasks requiring administrative privileges need privilege escalation.

**Solution:**

Use become in the playbook:

```yaml
- name: Install and configure Nginx
  hosts: webservers
  become: yes

  tasks:
    - name: Update apt cache
      apt:
        update_cache: yes

    - name: Install Nginx
      apt:
        name: nginx
        state: present

    - name: Start Nginx
      service:
        name: nginx
        state: started
        enabled: yes
```

**Verification:**

```bash
ansible-playbook -i inventory nginx.yml
ansible webservers -i inventory -m shell -a "systemctl status nginx" --become
```

**Learning:** Become allows Ansible to execute tasks with elevated privileges on managed nodes.

---

## 3. Ansible – Nginx Configuration and Role Practice

**Problem:**

While configuring Nginx using Ansible, application files and CSS needed to be deployed to the target Ubuntu machine.

**Implementation:**

- Created Ansible roles.
- Managed Nginx installation.
- Deployed web files.
- Configured CSS files.
- Verified Nginx service status.

**Verification:**

```bash
ansible-playbook -i inventory site.yml
systemctl status nginx
curl http://localhost
```

**Learning:** Ansible roles help organize reusable automation tasks and configuration files.

---

## Key Learnings

- Configure Ansible inventory.
- Understand controller and managed-node communication.
- Use SSH authentication.
- Apply privilege escalation.
- Automate Ubuntu package installation.
- Manage Nginx using Ansible roles.
