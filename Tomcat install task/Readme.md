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
  
