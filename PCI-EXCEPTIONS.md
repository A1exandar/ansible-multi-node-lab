# PCI-DSS exceptions

Profile: `xccdf_org.ssgproject.content_profile_pci-dss` (PCI-DSS v4.0.1 for RHEL 10), scanned with OpenSCAP.
Every rule listed here fails on purpose. Each exception names the risk it accepts and the controls that reduce it.

| Rule | Hosts | Reason | Compensating controls |
|---|---|---|---|
| `sudo_require_authentication` | all | The `ansible` service account runs unattended automation and needs sudo without a password prompt (`/etc/sudoers.d/ansible`, NOPASSWD). | Password locked (`passwd -l`); SSH login with a key only; `AllowUsers` limits who can log in over SSH; root cannot log in over SSH; auditd records every action with `auid=ansible`; sudo runs with `use_pty` and logs to `/var/log/sudo.log`. |
| `bios_enable_execution_restrictions` | all VMs | NX/XD is a firmware setting of the hypervisor (VMware Fusion), not of the guest OS. | The kernel enables NX by default on aarch64; checked by the virtualization host, not by Ansible. |
| `network_nmcli_permissions` | server1 | The check looks for a `.pkla` file from `polkit-pkla-compat`, which does not exist in RHEL 10 (polkit reads only JavaScript rules). | The same control is in `/etc/polkit-1/rules.d/10-nm-harden-access.rules`: every NetworkManager action by a non-root user needs admin authentication. |

Planned: replace the NOPASSWD exception with a sudo password stored in Ansible Vault.

Last review: 2026-11-27
