### * How to install `tomcat-apache` on host machine

### There are two method 
- [x] Method one install `one by one` <b>example<b> (download.yaml / extract.yaml / startup.yaml)
- [x] Method two `download / extract / startup` by one file 
## First try to `Method` one-
  ### 1. Create a yaml file named `download.yaml`, Because we need to download the tomcat-apache file first. so i create here a dynamic link -
  ```bash
  ---
- name: Download tomcat from server
  hosts: 192.168.1.10
  gather_facts: false
  become: yes
  vars:
          req_tomcat_ver: 9.0.50
          tomcat_url: https://downloads.apache.org/tomcat/tomcat-{{req_tomcat_ver.split('.')[0]}}/v{{req_tomcat_ver}}/bin/apache-tomcat-{{req_tomcat_ver}}.tar.gz
  
  tasks:
          - name: Download tomcat_apache on host
            debug: var=tomcat_url

  ```
  ![download_playbook](https://github.com/irezaul/Ansible/blob/main/Tomcat%20install%20task/ansible%20download%20link%20playbook.png)
  ## i create here a `dynamic` download link via `python` how see the images- 
  ![dynamic_link](https://github.com/irezaul/Ansible/blob/main/Tomcat%20install%20task/dynamic%20link%20generate.png)
  
  ###  Now how to Create this see image below -
  ![python](https://github.com/irezaul/Ansible/blob/main/Tomcat%20install%20task/pyhton%20lang.png)
  ![how_to_create](https://github.com/irezaul/Ansible/blob/main/Tomcat%20install%20task/how%20to%20create%20tomcat%20download%20link.png)
  
  ##  Now create a `Download` link playebook yaml
  ```bash
  ---
- name: Download tomcat from server
  hosts: 192.168.1.10
  gather_facts: false
  become: yes
  vars:
          req_tomcat_ver: 9.0.50
          tomcat_url: https://downloads.apache.org/tomcat/tomcat-{{req_tomcat_ver.split('.')[0]}}/v{{req_tomcat_ver}}/bin/apache-tomcat-{{req_tomcat_ver}}.tar.gz
  
  tasks:
          - name: Download tomcat_apache on host
            get_url:
                    url: "{{tomcat_url}}"
                    dest: /usr/local
```
![download.yaml](https://github.com/irezaul/Ansible/blob/main/Tomcat%20install%20task/tomcat%20download%20extract%20%26%20startup/show%20tomcat%20link%20by%20dynamic.png)
 > now save it & run playbook command
```bash
  ansible-playbook download_tomcat.yaml
  ```
  ![download_tomcat done](https://github.com/irezaul/Ansible/blob/main/Tomcat%20install%20task/tomcat%20download%20extract%20%26%20startup/tomcat%20dowload%20.png)
  > now go to the host machine on download destination & check by ls
  
  ![download done on host](https://github.com/irezaul/Ansible/blob/main/Tomcat%20install%20task/tomcat%20download%20extract%20%26%20startup/tomcat%20download%20done.png)
  
  ### 2. Now need to extract it - so create a yaml file named `extract_tomcat.yaml`
  ```bash
  ---
- name: Download tomcat from server
  hosts: 192.168.1.10
  gather_facts: false
  become: yes
  vars:
          req_tomcat_ver: 9.0.50
          tomcat_url: https://downloads.apache.org/tomcat/tomcat-{{req_tomcat_ver.split('.')[0]}}/v{{req_tomcat_ver}}/bin/apache-tomcat-{{req_tomcat_ver}}.tar.gz

  tasks:
          - name: Extracting tomcat tar file....
            unarchive:
                    src: /usr/local/apache-tomcat-{{req_tomcat_ver}}.tar.gz
                    dest: /usr/local
                    remote_src: yes
  ```
  ![extract_tomcat.yaml](https://github.com/irezaul/Ansible/blob/main/Tomcat%20install%20task/tomcat%20download%20extract%20%26%20startup/tomcat%20extract%20yaml.png)
  
  > save it & run again 
  ```bash
  ansible-playbook extract_tomcat.yaml
  ```
  > now goto host machine & check on your download destination extract done or not -
  
  ![extract_done](https://github.com/irezaul/Ansible/blob/main/Tomcat%20install%20task/tomcat%20download%20extract%20%26%20startup/tomcat%20extract%20done.png)
  
  > now you can see on extract file here a bin file & in bin file here many other file extract but we need only stastup.sh
  
  ![startup.sh](https://github.com/irezaul/Ansible/blob/main/Tomcat%20install%20task/tomcat%20download%20extract%20%26%20startup/host%20tomcat%20bin.png)
  
  ### 3. Now last option to startup tomcat on host machine - `Create` again a new `startup.yaml` file to startup `tomcat-apache`
  ```bash
  ---
- name: Download tomcat from server
  hosts: 192.168.1.11
  gather_facts: false
  become: yes
  vars:
          req_tomcat_ver: 9.0.50
          tomcat_url: https://downloads.apache.org/tomcat/tomcat-{{req_tomcat_ver.split('.')[0]}}/v{{req_tomcat_ver}}/bin/apache-tomcat-{{req_tomcat_ver}}.tar.gz

  tasks:
          - name: Starting Tomcat
            shell: nohup /usr/local/apache-tomcat-9.0.50/bin/startup.sh &
  ```
  ![startup_tomcat](https://github.com/irezaul/Ansible/blob/main/Tomcat%20install%20task/tomcat%20download%20extract%20%26%20startup/starting%20tomcat%20yaml.png)
  
  > save it & run it...
  ```bash
  ansible-playbook starting_tomcat.yaml
  ```
  - [x] JOB DONE...
  
  ### now to check on host machine is tomcat runing or not ~
  ```bash
  ps -ef | grep tomcat
  ```
  ![startup_done on host](https://github.com/irezaul/Ansible/blob/main/Tomcat%20install%20task/tomcat%20download%20extract%20%26%20startup/tomcat%20run%20successfully.png)
  
  ### Successfully run on host - check by `hostip:8080`
  ![run_on_host](https://github.com/irezaul/Ansible/blob/main/Tomcat%20install%20task/tomcat%20download%20extract%20%26%20startup/successfully%20run%20on%20host.png)
  
  
  ## Method two in one file `multiple task` execute 
  > Here the command
  ```bash
  ---
- name: Download tomcat from server
  hosts: 192.168.1.11
  gather_facts: false
  become: yes
  vars:
          req_tomcat_ver: 9.0.50
          tomcat_url: https://downloads.apache.org/tomcat/tomcat-{{req_tomcat_ver.split('.')[0]}}/v{{req_tomcat_ver}}/bin/apache-tomcat-{{req_tomcat_ver}}.tar.gz

  tasks:
          - name: Download tomcat_apache on host...
            get_url:
                    url: "{{tomcat_url}}"
                    dest: /usr/local

          - name: Extracting tomcat tar file....
            unarchive:
                    src: /usr/local/apache-tomcat-{{req_tomcat_ver}}.tar.gz
                    dest: /usr/local
                    remote_src: yes

          - name: Starting Tomcat on host...
            shell: nohup /usr/local/apache-tomcat-9.0.50/bin/startup.sh &
  ```
  > save it & run 
  
  # Enjoy 
  
  [i love master-academy](https://www.facebook.com/masteracadmy) | 
  [always helpful group](https://www.facebook.com/groups/codingbootcampbd)
  
  
  # Now how to install Apache-Tomcat by a role 
  > create or init a role on your destination or patch then type a command to create role directory 
  ```bash
  ansible-galaxy init tomcat
  ```
  > then go to role folder what you name define ( so i create here - `etc/ansible/playbook/roles/tomcat/tasks/`) then open the `main.yaml` file by vim editor. 
  ```bash
  vim main.yaml
  ```
  #### Here now define the task what we do or what we need . I'm shared my task see 
  ```bash
  - name: Updateing repos
  apt:
          name: "*"
          state: latest
- name: Install java require
  apt:
          name: openjdk-8-jre
          state: present

- name: Download tomcat package
  command: "wget https://downloads.apache.org/tomcat/tomcat-9/v9.0.50/bin/apache-tomcat-9.0.50.tar.gz"
  args:
     chdir: /tmp/

- name: Extract Tomcat archive
  command: tar zxvf /tmp/apache-tomcat-9.0.50.tar.gz -C /opt/
  args:
     chdir: /tmp/
#- name: Stop Tomcat
#  command: nohup /opt/apache-tomcat-8.5.40/bin/shutdown.sh

- name: Start Tomcat
  command: nohup /opt/apache-tomcat-9.0.50/bin/startup.sh &
  ```
  ![tomcat tasks yaml file](https://user-images.githubusercontent.com/77927449/126906610-62a9ad22-d22a-4307-9cbf-82155e9a8f11.png)

  ### Now go back to the role folder what we init here i create a master.yaml 
  ![master yaml](https://user-images.githubusercontent.com/77927449/126906715-ab770bc0-33b1-42bf-86b9-730feb6e729e.png)
> now edit this file with vim editor
  ```bash
  ---
- hosts: 192.168.1.109
  gather_facts: true
  become: true
  become_method: sudo
  roles:
          - tomcat
  ```
  ![Master yaml edit](https://user-images.githubusercontent.com/77927449/126906814-77e178e6-f48f-45e7-bdf4-c58ac8a66fb8.png)
  
#### Now we're ready to play this file ...
  
 ```bash
  ansible-playbook master.yaml
  ```
  ![create tomcat_role done](https://user-images.githubusercontent.com/77927449/126906889-19a127df-4648-48da-93a5-908b61c8efd0.png)

  
  
