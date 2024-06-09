# AWX Installation and Configuration Playbook

This Ansible playbook automates the installation and configuration of AWX using k3s on a target host. The playbook is designed to be configurable and reusable, leveraging variables and conditions to ensure flexibility.

## Prerequisites

The playlist will check for these that it can but honestly how can you run it without these?

- Ansible installed on the control machine.
- SSH access to the target host(s) with sudo privileges.
- Python and pip installed on the target host(s).
- Required Ansible collections installed.

## Vault Variables

Ensure you have a `vault.yml` file containing the following encrypted variables:
A password is required when it asks.
Create the vault in the same folder with ```ansible-vault create vault.yml```
You can edit it with ```ansible-vault edit vault.yml```

```yaml
become_passwordV: "your_sudo_password"
awx_host_referenceV: "awx"
awx_hostV: "awx.yourdomain.com" # FQDN of your awx host that the self-signed cert will use
dns_serverV: "8.8.8.8" # set your local dns that can resolve your computer names
node_host4V: "192.168.1.10 yourhostname" # awx host ipv4 address
node_host6V: "fe80::1 yourhostname" # ipv6 host address if used - i haven't tested leaving blank yet
awx_postgres_passwordV: "your_postgres_password" # custom password for postres
awx_admin_passwordV: "your_admin_password" # set your admin password for the web login
timeout_valueV: 600  # 10 minutes in seconds
delay_per_retryV: 20 # 20 seconds between retry
repo_my_git_userV: "yourusername" # your git username assuming you make your own fork
repo_upstream_userV: "kurokobo" # The person that deserves the credit, I just built the playlist.
```

## Notes

It will verify and enable SELinux if disabled, update firewalld rules needed, and take care of everything for you.
The deployment will be a basic install that can see computers on your local network with the dns forward.
I'll eventually break this down into roles to be managed better.

Run the playbook with ```ansible-playlist -i inventory.yaml install-k3s-operator-awx.yaml --ask-vault-pass```

### Explanation:
- **Prerequisites**: Lists the requirements for running the playbook.
- **Vault Variables**: Explains the variables stored in the `vault.yml` file.
- **Inventory File**: Provides an example of how to configure the inventory.
- **Playbook Structure**: Gives an overview of the main tasks and their purposes.
- **Key Tasks**: Provides detailed examples of critical tasks within the playbook.
- **Running the Playbook**: Instructions for executing the playbook.
- **Contributing**: Encourages users to contribute to the project.
- **License**: States the project's license.