# Ansible Nginx Demo

Ansible control node setup managing multiple remote servers from a single machine.

## Features
- Nginx installation and configuration
- Handlers - restart only when config changes
- Variables - no hardcoded values
- Ansible Vault - encrypted sensitive data
- Roles - organized reusable structure

## Structure
inventory          # server inventory
site.yml           # main playbook
roles/
nginx/
tasks/         # installation and config tasks
handlers/      # service restart handler
vars/          # role variables

## Inventory
Servers are defined in the inventory file by group.
Add as many servers as needed — Ansible will configure all of them simultaneously with one command:

```ini
[webservers]
server1_ip
server2_ip

[dbservers]
db1_ip
db2_ip

[all:vars]
ansible_ssh_private_key_file=~/.ssh/your_key.pem                      # use your key here
ansible_ssh_common_args='-o StrictHostKeyChecking=no'

[webservers:vars]
ansible_user=ec2-user                                                 # use your username here

[dbservers:vars]
ansible_user=ubuntu                                                   # use your username here

## Usage
```bash
# Run against all servers
ansible-playbook -i inventory site.yml --ask-vault-pass

# Run against specific group only
ansible-playbook -i inventory site.yml --limit webservers --ask-vault-pass

# Test connectivity to all servers
ansible -i inventory all -m ping
```