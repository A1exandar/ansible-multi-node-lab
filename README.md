# Ansible multi-node lab

A home lab for practising Linux administration and automation: RHEL-family and Debian servers managed from one control node with Ansible. Every change goes through a playbook and Git.

## Nodes

| Host | OS | Role |
|---|---|---|
| `server1` | RHEL 10 | control node, local package repo |
| `server2` | RHEL 10 | application server |
| `mon1` | AlmaLinux | logging and monitoring |
| `deb1` | Debian 12 | second distribution family |

## Inventory groups

| Group | Hosts | Used for |
|---|---|---|
| `rhel` | server1, server2, mon1 | dnf, SELinux, firewalld |
| `debian` | deb1 | apt |
| `monitoring` | mon1 | monitoring stack |

## Playbooks

| Playbook | What it does |
|---|---|
| `vmtools.yml` | installs and enables open-vm-tools on both families (`ansible_os_family`, `ansible.builtin.package`) |

## Usage

~~bash
ansible all -m ping -o
ansible-playbook vmtools.yml
~~

## Roadmap

1. Inventory and cross-distro playbooks (done)
2. Security baseline: OpenSCAP (PCI-DSS profile), auditd, AIDE
3. Central logging and monitoring on `mon1`
4. Patching, backups with restore tests, internal CA
5. Incident simulations with short write-ups

Write-ups of the individual tasks: [a1exnotes.com/labs](https://a1exnotes.com/labs/)
