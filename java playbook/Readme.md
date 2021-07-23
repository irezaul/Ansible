## How to create `JAVA` playbook (yaml file)

```bash
--- 
- name : Installing Java
  hosts: 192.168.x.x
  gather_facts: flase
  become: yes
  tasks:
         - name: updating repos
           apt:
                  name: "*"
                  state: latest
         - name: Install required Java
           apt:
                  name: openjdk-8-jre
                  state: present
```
![java_template](https://github.com/irezaul/Ansible/blob/main/java%20playbook/Java%20playbook.png)

- [x] save it by any name & .yaml file

### Install or execute java by `playbook_yaml` file
```bash
ansible-playbook filename.yaml
```
![java run command](https://github.com/irezaul/Ansible/blob/main/java%20playbook/Install%20java%20by%20playbook_yaml.png)

### Now check on `host` machine java installed or not
```bash
java --version
```
![install java done](https://github.com/irezaul/Ansible/blob/main/java%20playbook/Java%20install%20done%20on%20host.png)
