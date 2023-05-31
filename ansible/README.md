# Ansible Playbook to Setup a Linux Desktop

The playbook aims to automate installation of various tools needed for software development.

## Prerequisites

- [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html#installing-and-upgrading-ansible)
- Ansible Packages
  - `ansible-galaxy collection install community.general`

## Usage

```sh
ansible-playbook main.yml -i inventory.yml --ask-become-pass
```

Also check out the tags on each role in the `main.yml` file. \
They allow to customize which tools are being installed, e.g.:

```sh
ansible-playbook main.yml -i inventory.yml --ask-become-pass --tags kubernetes,rust
```
