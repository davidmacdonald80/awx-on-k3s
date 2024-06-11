# AWX Installation and Configuration Playbook

This Ansible playbook automates the installation and configuration of AWX using k3s on a target host. The playbook is designed to be configurable and reusable, leveraging variables and conditions to ensure flexibility.

built and tested on almalinux 9 machines.

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
delay_per_retryV: 20 # 20 seconds between retry it will divide the timout value by this.
repo_my_git_userV: "davidmacdonald80" # change if you fork it
```

## Notes

It will verify and enable SELinux if disabled, update firewalld rules needed, and take care of everything for you.
The deployment will be a basic install that can see computers on your local network with the dns forward.
I'll eventually break this down into roles to be managed better.

Run the playbook with ```ansible-playlist -i inventory.yaml install-k3s-operator-awx.yaml --ask-vault-pass```

## rundown of the playbook tasks
- dnf installs all latest updates, reboots if need and continues
- created k3s_awx service and applies to firewalld public zone
- adds default IP ranges used for k3s to trusted zone
- makes sure SELinux is enabled (doesn't matter if permissive or not), sets to permissive if disabled
- check for latest version of k3s
- install k3s if it's not already
- create coredns configmap to allow forwarding to your local dns
- Apply coredns configmap if needed
- apply patch to make sure coredns always uses the new configmap
- Copy k3s.yaml to ~/.kube/config
- add kubectl bash completion to /etc/bashrc
- add export KUBECONFIG to /etc/bashrc and activate changes
- get latest AWX-Operator version and set in kustomization (takes several tasks)
- Apply latest AWX-Operator and wait for it to be ready {{ vault_timeout_value }}
- create cert for awx
- create updated kustomization and CRs with vault variables
- remove files used from earlier tasks
- create persistant storage folders on host and set chown and chmod
- deploy AWX
- wait up to {{ vault_timeout_value }} to verify AWX has been deployed successfull
- reboot if needed


### Explanation:
- **Prerequisites**: Lists the requirements for running the playbook.
- **Vault Variables**: Explains the variables stored in the `vault.yml` file.
- **Inventory File**: Provides an example of how to configure the inventory.
- **Playbook Structure**: Gives an overview of the main tasks and their purposes.
- **Key Tasks**: Provides detailed examples of critical tasks within the playbook.
- **Running the Playbook**: Instructions for executing the playbook.
- **Contributing**: Encourages users to contribute to the project.
- **License**: States the project's license.