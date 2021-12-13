# Ansible conditionals

Conditionals are used to determine whether a task should be run or not. They are used in the `when` and `with_` keywords. They are similar to `if` statements in other languages.


```yml
# playbook-conditionals1.yml
---
- name: Install NGINX
  hosts:
  tasks:
  - name: Install NGINX on Debian
    apt:
      name: nginx
      state: present
```

```yml
# playbook-conditionals2.yml
---
- name: Install NGINX
  hosts:
  tasks:
  - name: Install NGINX on Redhat
    dnf:
      name: nginx
      state: present
```

```yml
# playbook-conditionals-when.yml
---
- name: Install NGINX
  hosts:
  tasks:
  - name: Install NGINX on Debian
    apt:
      name: nginx
      state: present
    when: ansible_os_family == 'Debian' and ansible_distribution_version >= '8'
  - name: Install NGINX on Redhat
    dnf:
      name: nginx
      state: present
    when: ansible_os_family == 'Redhat' or ansible_os_family == 'CentOS'
```
Conditional statements can be used in loops and in other conditional statements. The following example shows how to use conditional statements in loops.  The `when` and `with_` keywords can be used to assign a value to a variable.

```yml
# playbook-conditionals-loops.yml
---
- name: Install Softwares
  hosts: all
  vars:
    packages:
      - name: nginx
        required: Yes
      - name: php
        required: Yes
      - name: mysql
        required: Yes
      - name: apache
        required: No
  tasks:
  - name: Install {{ item.name }} on Debian
    apt:
      name: "{{ item.name }}"
      state: present
    when: item.required == 'Yes' and ansible_os_family == 'Debian' and ansible_distribution_version >= '8' 
    loop: "{{ packages }}"
```

Conditionals and register -  here below an example when a service is down and sends an email to the administrator.

```yml
# playbook-conditionals-register.yml
---
- name: Check if service is down
  hosts: all
  tasks:
  - name: Check if service is down
    service:
      name: "{{ item.name }}"
      state: "{{ item.state }}"
    with_items: "{{ services }}"
    register: service_status
  - name: Send email
    email:
      to: "{{ admin_email }}"
      subject: "{{ item.name }} is down"
      body: "{{ item.name }} Service is down"
    when: service_status.changed == 1
    with_items: "{{ services }}"
    loop: "{{ service_status.results }}"
```
*see [playbook-conditionals-register.yml](playbook-conditionals-register.yml)*

*see also:* [Conditionals in Ansible](https://docs.ansible.com/ansible/latest/reference_appendices/playbooks_conditional.html)




- The given playbook attempts to start mysql service on all_servers. Use the when condition to run this task if the host (ansible_host) is the database server. Refer to the inventory file to identify the name of the database server.

```ini
# We have created a group for web servers. Similarly create a group for database servers named 'db_servers' and add db1 server to it
# --------------------------------
# Sample Inventory File

# Web Servers
web1 ansible_host=server1.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web2 ansible_host=server2.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!
web3 ansible_host=server3.company.com ansible_connection=ssh ansible_user=root ansible_ssh_pass=Password123!

# Database Servers
db1 ansible_host=server4.company.com ansible_connection=winrm ansible_user=administrator ansible_ssh_pass=Password123!

[web_servers]
web1
web2
web3

[db_servers]
db1
```

```yml
# playbook-mysql-started.yml
---
-
    name: 'Execute a script on all web server nodes'
    hosts: all_servers
    tasks:
        -
            service: 'name=mysql state=started'
            # solution
            when: 'ansible_host=="server4.company.com"'
```
*see [playbook-mysql-started.yml](playbook-mysql-started.yml)*
*see also:* [Conditionals in Ansible](https://docs.ansible.com/ansible/latest/playbooks_best_practices.html#conditionals)

- The playbook has a variable defined - age. The two tasks attempt to print if I am a child or an Adult. Use the when conditional to print if I am a child or an Adult based on weather my age is < 18 (child) or >= 18 (Adult).

```yml
# playbook-age.yml
---
-
    name: 'Am I an Adult or a Child?'
    hosts: localhost
    vars:
        age: 25
    tasks:
        -
            command: 'echo "I am a Child"'
            # solution 1
            when: 'age < 18'
        -
            command: 'echo "I am an Adult"'
            # solution 2
            when: 'age >= 18'
```
*see [playbook-age.yml](playbook-age.yml)*


- The given playbook attempts to add an entry into the /etc/resolv.conf file for nameserver. First, we run a command using the shell module to get the contents of /etc/resolv.conf file and then we add a new line containing the name server data into the file. However, when this playbook is run multiple times, it keeps adding new entries of same line into the resolv.conf file. 
1. Add a register directive to store the output of the first command to variable command_output
2. Then add a conditional to the second command to check if the output contains the name server (10.0.250.10) already. Use command_output.stdout.find(<IP>) == -1
> Note: A better way to do this would be to use the lineinfile module. This is just for practice.
> Note: shell and command modules are similar in that they are used to execute a command on the system. However shell executes the command inside a shell giving us access to environment variables and redirection using >>

```yml
# playbook-resolv.yml
---
-
    name: 'Add name server entry if not already entered'
    hosts: localhost
    tasks:
        -
            shell: 'cat /etc/resolv.conf'
            # solution 1
            register: command_output
        -
            shell: 'echo "nameserver 10.0.250.10" >> /etc/resolv.conf'
            # solution 2
            when: 'command_output.stdout.find("10.0.250.10") == -1'
```
*see [playbook-resolv.yml](playbook-resolv.yml)*
