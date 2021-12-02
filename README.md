# WordPress with MySQL on a separate node on Ubuntu 18.04/20.04

The playbook is designed with one ansible control node and two hosts in mind (one for the Wordpress + LAMP and the another one for the MySQL DB). Also for the purpose of this test, you need to add a firewall rule that would allow all "All traffic" during the AMIs creation process.

## Considering you have freshly provisioned AWS nodes, you should consider:

- Install latest updates and upgrades on your ansible control node and hosts
```shell
sudo apt update -y
sudo apt upgrade -y
sudo apt install python
```
- Install Ansible on your designated control node
```shell
sudo apt install ansible
```
- Add your child hosts into the ansible host file. You can get a hold of your child servers IPs using ifconfig (search for your eth0 device IP address)
```shell
ip address show
```
## Set up a connection between master and child nodes

- Generate new public key, or use an exiting one by running the below command
 ```shell
 ssh-keygen
 ```
- Display the content of your newly generated public key and copy it to the clipboard
 ```shell
cat ~/.ssh/id_rsa.pub
```
- On each of your ansible hosts paste the content of your clipboard (the ssh key) on a new line into ~/.ssh/authorized_keys
```shell
vim ~/.ssh/authorized_keys
```
- Add default interpreter in the ansible.cfg (paste "interpreter_python = /usr/bin/python3" under [defaults] section in the ansible.cfg)
```shell
sudo vim ansible.cfg
```
- Test connection between master and chile hosts
```shell
ansible wordpress -m ping
```
- Clone the git repository
```shell
git clone https://github.com/georgi-nachev/ansible-wordpress.git
``` 
- Run the ansible playbook 
```shell
ansible-playbook playbook.yml
```
