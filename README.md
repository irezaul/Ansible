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
