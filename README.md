# Add Windows VM to Domain Playbook

This Ansible playbook is designed to add a Windows VM to a domain and then reboot the VM to complete the process.

## Prerequisites

- Ansible installed on a control machine (Linux/Unix).
- Properly configured inventory file with the target Windows VM.
- The Windows VM must be accessible from the control machine.
- The necessary credentials to join the domain.

## Playbook Details

### Playbook: `add_vm_domain_win.yml`

This playbook performs the following tasks:
1. Joins the Windows VM to the specified domain.
2. Reboots the Windows VM to apply the changes.
3. Waits for the reboot to complete.

### Playbook Content

```yaml
---
- name: Add Win VM to domain 
  hosts: win
  tasks:
    - name: Join VM to domain
      win_domain_membership:
        dns_domain_name: SLBPOC.com  # Corrected the domain name format
        hostname: t5-win-1
        domain_admin_user: Sac-domjoin
        domain_admin_password: Lumi@389#
        # domain_ou_path: "OU=Windows,OU=Servers,DC=ansible,DC=vagrant"
        state: domain   
      register: domain_output

    - name: Reboot Server
      win_shell: shutdown /r /f /t 5

    - name: Wait for reboot to complete
      pause: 
        minutes: 2
