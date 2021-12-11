# Ansible.cfg file

This file is used to store some configuration details rather than storing in the inventory files. By setting an ansible.cfg file we can omit -i inventory_file.

Ansible checks `ansible.cfg` file in the order below

1. `ANSIBLE_CONFIG` environment variable

2. `./ansible.cfg` file in the current directory

3. `~/.ansible.cfg` in users home directory

4. `/etc/ansible/ansible.cfg` system-wide config file
