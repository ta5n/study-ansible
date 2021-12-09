# Playbook.yml file

This file is a list of configurations to be run on listed environments. Every list item has these key-values.`tasks` section is an ordered list!

```yaml
# playbook.yml
- name: Play 1
  hosts: localhost
  tasks:
    - name: Execute a command
      command: date

    - name: execute a script
      script: test_script.sh

    - name: install httpd service
      yum:
        name: httpd
        state: present

    - name: start web server
      service:
        name: httpd
        state: started
```

host should be defined in the `inventory` file:

```ini
# inventory file
localhost

server1.company.com
server2.company.com

[mail]
server3.company.com
server4.company.com

[db]
server5.company.com
server6.company.com

[web]
server7.company.com
server8.company.com
```
