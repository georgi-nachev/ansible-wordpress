# For the purpose of this test, we'll add a firewall rule that would allow all "All traffic" AMI creation.

# The playbook is designed with one ansible control node and two hosts in mind, one for the Wordpress + LAMP and the other one for the MySQL DB

# Considering you have freshly provisioned AWS nodes, you should consider:
- Install latest updates and upgrades as well as your Ansible Control Node and hosts
	sudo apt update -y
	sudo apt upgrade -y
	sudo apt install net-tools
	sudo apt install python
- Make sure you have Ansible installed on your master server
	sudo apt install ansible
- Add your child hosts into the ansible host file. You can get a hold of your child servers IPs using ifconfig (search for your eth0 device IP address)
- Set up a connection between master and child nodes
 - Generate new public key, or use an exiting one by running the below command
 ```shell
 ssh-keygen
 ```
 - Display the content of your newly generated public key and copy it to the clipboard
 ```shell
cat ~/.ssh/id_rsa.pub
```
- On each of your child nodes paste the content of your clipboard (the ssh key) on a new line into ~/.ssh/authorized_keys
- Add default interpreter in the ansible.cfg
	paste this line "interpreter_python = /usr/bin/python3" under [defaults] section in the ansible.cfg
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