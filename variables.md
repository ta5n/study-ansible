# Ansible Variables

Used to store and pass information such as username, password, host etc, amoung playbook, inventory and targets.

Jinja2 templating engine styling ( double curly braces surround the variable name ) is used. Using variables in playbook files:

  **{{ variable_name }}**

variables can be defined in inventory file or in a seperate yaml file like below:

```ini
; inventory file
Web http_port=8081 snmp_port=161-162 inter_ip_range=192.0.2.0
```

```yml
# variable file - web.yml

http_port: 8081
snmp_port: 161-162
inter_ip_range: 192.0.2.0
```
- using variables in playbook file
```yml
# playbook-web-firewall.yml
---
-
  name: set firewall conf
  hosts: web
  tasks:
  - firewalld:
    service: https
    permanent: true
    state: enabled

- firewalld:
    port: '{{ http_port }}'/tcp
    permanent: true
    state: disabled

- firewalld:
    port: '{{ snmp_port }}'/udp
    permanent: true
    state: disabled
- firewalld:
    source: '{{ inter_ip_range}}'/24
    Zone: internal
    state: enabled
``` 


- The playbook is used to update name server entry into resolv.conf file on localhost. The name server information is also updated in the inventory file as a variable nameserver_ip. Refer to the inventory file. Replace the ip of the name server in this playbook to use the value from the inventory file, so that in the future if you had to make any changes you simply have to update the inventory file.

```ini
; inventory file
localhost ansible_connection=localhost nameserver_ip=10.1.250.10
```

```yml	
# playbook-update-nameserver.yml
---
-
    name: 'Update nameserver entry into resolv.conf file on localhost'
    hosts: localhost
    tasks:
        -
            name: 'Update nameserver entry into resolv.conf file'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver 10.1.250.10'
# solution to replace the ip of the name server in this playbook to use the value from the inventory file
#                line: 'nameserver {{ nameserver_ip }}' 
                when: '{{ nameserver_ip }}'
                state: present
```

- We have added a new task to disable SNMP port in the playbook. However the port is hardcoded in the playbook. Update the inventory file to add a new variable snmp_port and assign the value used here. Then update the playbook to use value from the variable. Remember to use curly braces around the variable name.

```ini
; inventory file
localhost ansible_connection=localhost nameserver_ip=10.1.250.10
; solution
localhost ansible_connection=localhost nameserver_ip=10.1.250.10 snmp_port=160-161
```

```yml
# playbook-update-snmp.yml
---
-
    name: 'Update nameserver entry into resolv.conf file on localhost'
    hosts: localhost
    tasks:
        -
            name: 'Update nameserver entry into resolv.conf file'
            lineinfile:
                path: /etc/resolv.conf
                line: 'nameserver {{ nameserver_ip }}'
        -
            name: 'Disable SNMP Port'
            firewalld:
                port: 160-161
# solution to replace snmp port in this playbook to use the value from the inventory file                
#                port: {{ snmp_port }}
                permanent: true
                state: disabled
```
- We are printing some personal information to the screen. We would like to move the car_model, country_name and title to a variable defined at the play level. Create three new variables (car_model, country_name and title) under the play and move the values over. Use the variables in the task.

```yml
# playbook-define-vars.yml
---
-
    name: 'Update nameserver entry into resolv.conf file on localhost'
    hosts: localhost
# solution to move the car_model, country_name and title to a variable defined at the play level
    vars:
        car_model: 'BMW 318i'
        country_name: Turkey
        title: 'Software Engineer'    
    tasks:
        -
            name: 'Print my car model'
            command: 'echo "My car''s model is {{ car_model }}"'
        -
            name: 'Print my country'
            command: 'echo "I live in the country of {{ country_name }}"'
        -
            name: 'Print my title'
            command: 'echo "I work as a {{ title }}"'
```
