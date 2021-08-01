# `Ansible` How to install on your VM machine ubuntu 

#### 1. update first your apt
```python
sudo apt-get update
```
#### 2. `Ansible` use this command for install ( `It only needs to be installed on the master-PC `)
``` phython
sudo apt-get install ansible
```
#### 3. Next need to configure password less SSH: First need to create a `SSH key pair` on AnsibleMaster
```bash
ssh-keygen
```
#### this keyfile generate on `root` folder 
```bash
cd root/.ssh
```
![rsa_key](https://user-images.githubusercontent.com/77927449/125205038-18961380-e2a2-11eb-8aef-57a7c89cef60.png)
> (press enter for all options)
#### 4. To copy the public key type command
```bash
cat .ssh/id_rsa.pub
```

![rsa_pub](https://user-images.githubusercontent.com/77927449/125205063-3794a580-e2a2-11eb-9c46-a2058bb5b088.png)


#### 5. and select the key to copy In all the host nodes type command
```bash
vim .ssh/authorized_keys
```
> and paste by right clicking From AnsibleServer ssh to all the nodes once then logout: ssh 19X.16X.1.1 

### * Configuring Host File
> As root user the host file is under `/etc/ansible/hosts`
> Now you can edit and save the file using vim or nano ( text editor)
`vim /etc/ansible/hosts`
#### 6. Write here your `IP addresses` of the hosts at the bottom of the file in this format and save and exit vim
![ip insert](https://user-images.githubusercontent.com/77927449/125205280-4760b980-e2a3-11eb-8a38-7fe8dd3ff3e6.png)

#### 7. `Ad-hoc` Commands & Modules in Ansible
> Example 1: Use the ping module to check if a host can be pinged
```bash
ansible ip-192.168.x-x -m ping
```
![ansible_ip](https://user-images.githubusercontent.com/77927449/125313677-49845000-e357-11eb-8729-0313d0c7c2bc.png)
> Example 2: Create a new user by using the user module
```bash
ansible ip-172-31-12-148 -m user -a "name=demouser state=present"
```
![ansible demo user](https://user-images.githubusercontent.com/77927449/125313717-56a13f00-e357-11eb-8c7e-2776c4de1196.png)

#### 8. `Ad-hoc` Commands & Modules in Ansible (cont)
> Check if demouser has been created in host
```bash
id demouser
```
#### 9. Install a package on the host machine using apt module
```bash 
ansible ip-172-31-12-148 -m apt -a "name=finger state=present update_cache=true"
```

#### 10. Check on the host machine if `finger` was installed by typing finger To run a module on all the hosts use “all” instead of IP address
```bash
ansible all -m apt -a "name=finger state=present update_cache=true"
```
##### * Create and Execute a Playbook

#### 11. Change directory to /etc/ansible: 
```bash
cd /etc/ansible
```
#### 12. Use vim editor to write a playbook: 
```bash
vim demoplaybook.yaml
```
#### 13. Type this command to execute the play: 
```bash
ansible-playbook demoplaybook.yaml
```
### * if you get any `error` like this don't panic just follow the instruction-
> E: Could not get lock /var/lib/dpkg/lock – open (11: Resource temporarily unavailable)
E: Unable to lock the administration directory (/var/lib/dpkg/), is another process using it?

##### check the process (package manager for handling software). Use this command:
```bash 
ps aux | grep -i apt
```
> If you see that apt is being used by a program like `apt.systemd.daily update`,
#### Method 1
```bash
sudo killall apt apt-get
```
#### Method 2 
```bash 
sudo lsof /var/lib/dpkg/lock
sudo lsof /var/lib/apt/lists/lock
sudo lsof /var/cache/apt/archives/lock
```
#### You can now safely remove the lock files using the commands below:
```bash
sudo rm /var/lib/apt/lists/lock
sudo rm /var/cache/apt/archives/lock
sudo rm /var/lib/dpkg/lock
```
#### [ ] now install your any packages 

### * Here a demo install java by playbook -
![Java playbook](https://user-images.githubusercontent.com/77927449/126518037-f4664e0a-0079-468d-bb33-2c9494a848ab.png)

> install process done
![Install java by playbook_yaml](https://user-images.githubusercontent.com/77927449/126518122-0f9ff347-be00-40f1-bb73-df161ecb7b73.png)
![Java install done on host](https://user-images.githubusercontent.com/77927449/126518439-3b8ff114-3554-40ce-b178-ab00b4b1f3fa.png)



[install tomcat apache](https://github.com/irezaul/Ansible/tree/main/Tomcat%20install%20task)
