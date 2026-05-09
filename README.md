# Ansible Infrastructure Demo

Ansible control node setup managing multiple remote servers with a complete monitoring stack.

## Architecture
```
Control Node (Mac/Linux)
└── Ansible
    │
    ├── Web Servers (x2)
    │   ├── nginx
    │   └── node_exporter
    │
    └── Monitoring Server (x1)
        ├── node_exporter
        ├── Prometheus
        └── Grafana
```

## Features
- Nginx installation and configuration
- Handlers - restart only when config changes
- Variables - no hardcoded values
- Ansible Vault - encrypted sensitive data
- Roles - organized reusable structure
- Prometheus - metrics collection from all servers
- Grafana - visualization dashboard (Node Exporter Full - ID 1860)

## Roles
| Role | Description |
|---|---|
| nginx | Install, configure and start nginx |
| node_exporter | Deploy Prometheus Node Exporter |
| prometheus | Install and configure Prometheus server |
| grafana | Install Grafana and connect to Prometheus |

## Inventory
Servers are defined by group — add as many as needed:

```ini
[webservers]
server1_ip
server2_ip

[monitoring]
monitoring_server_ip

[webservers:vars]
ansible_user=ec2-user

[monitoring:vars]
ansible_user=ec2-user

[all:vars]
ansible_ssh_private_key_file=~/.ssh/your_key.pem
```

## Usage
```bash
# Test connectivity to all servers
ansible -i inventory all -m ping

# Deploy everything
ansible-playbook -i inventory site.yml

# Deploy with vault encrypted variables
ansible-playbook -i inventory site.yml --ask-vault-pass

# Target specific group only
ansible-playbook -i inventory site.yml --limit webservers
```

## Monitoring
- Prometheus UI: `http://monitoring_server_ip:9090`
- Grafana UI: `http://monitoring_server_ip:3000`
- Node Exporter metrics: `http://any_server_ip:9100/metrics`
