# WordPress Deployment with Ansible

Deploys WordPress on an AlmaLinux server using Apache (HTTP + HTTPS), MariaDB and PHP, split into three roles.

## Project layout
- `site.yml` - main playbook, runs the roles in order
- `hosts` - inventory (group `wordpress_servers`)
- `ansible.cfg` - inventory and vault settings
- `group_vars/wordpress_servers/vars.yml` - shared variables (site, database, admin, paths, plugins)
- `group_vars/wordpress_servers/vault.yml` - encrypted passwords (Ansible Vault)
- `roles/stack_install` - installs Apache, MariaDB, PHP and WP-CLI
- `roles/stack_config` - app user, WordPress directory, PHP settings, SSL certificate, Apache virtual hosts, SELinux fix, firewall, services
- `roles/wordpress` - creates the database and user, installs WordPress and plugins with WP-CLI

## Requirements
- Ansible on the control machine
- AlmaLinux server reachable over SSH with sudo
- Collections: `community.mysql`, `ansible.posix`

## Setup
1. Put the server IP and SSH user in `hosts`.
2. Create `.vault_pass` containing your vault password.
3. Store passwords in `vault.yml` and encrypt it with `ansible-vault encrypt`.
4. Test the connection: `ansible wordpress_servers -m ping`

## Run
```
ansible-playbook site.yml --syntax-check
ansible-playbook site.yml
ansible-playbook site.yml    # second run to check idempotence
```
Then open `http://SERVER_IP` and `https://SERVER_IP`.

## Troubleshooting summary
- **Error introduced:** (misspelled package name in `stack_install`)
- **How I found it:** (the Ansible error message named the problem)
- **Fix:** (corrected it and reran successfully)
