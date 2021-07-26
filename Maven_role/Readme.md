### How to create Maven Role -
> create a role or init directory on your ansible folder
>Like-

└── roles
    ├── maven
    │   └── tasks
    │       └── main.yamll
    
![maven_init](https://user-images.githubusercontent.com/77927449/126939199-076ee995-59ef-4d97-aec9-6316fd9c1f99.png)

#### go to the maven tasks folder & open main.yaml file by vim editor
> type the tasks- 

```bash
- name: Update apt-get repo
  apt: update_cache=yes force_apt_get=yes

- name : Install Maven
  apt:
        name: maven
        state: present
```
![maven_role_task](https://github.com/irezaul/Ansible/blob/main/Maven_role/Maven_role_task.png)
    
#### Now go back to playbook folder & create a maven_install.yaml
> type here host & role name
```bash
---
- hosts: 192.168.1.109
  become: true
  roles:
          - maven
 ```
 ![maven_role](https://github.com/irezaul/Ansible/blob/main/Maven_role/maven_role.png)
 
 ### Now run your playbook 
 ```bash
 ansible-playbook maven_install.yaml
 ```
 ![playbook_run](https://github.com/irezaul/Ansible/blob/main/Maven_role/Maven_playbook_execute.png)
 
 #### check on your host machine maven install or not 
 ```bash
 mvn --version
 ```
 ![maven_version](https://github.com/irezaul/Ansible/blob/main/Maven_role/Maven_version.png)
