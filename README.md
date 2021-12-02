# WordPress with MySQL on a separate node on Ubuntu 18.04/20.04

The playbook is designed with one ansible control node and two hosts in mind (one for the Wordpress + LAMP and the another one for the MySQL DB). Also for the purpose of this test, you need to add a firewall rule that would allow all "All traffic" during the AMIs creation process.

## Considering you have freshly provisioned AWS nodes:

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
- To set up the connection between the master and child nodes you need to generate a new public key, or use an exiting one by running the below command
 ```shell
 ssh-keygen
 ```
- Display the content of your public key and copy it to the clipboard
 ```shell
cat ~/.ssh/id_rsa.pub
```
- On each of your ansible hosts paste the content of your clipboard (the ssh key) on a new line into your authorized_keys file
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

## Settings:

- `php_extensions`:  All necessary php modules for a clean WordPress installation. No need to change this variable, but you might want to adapt it to your custom needs by extending it with additional modules.  
- `database_name`: Your WordPress database name.
- `database_user`: Your WordPress database username.
- `database_user_password`: The password for your WordPress database username.
- `database_ip`: The IP Address of your Database server (AMI)
- `wordpress_ip`: The IP Address of your WordPress server (AMI)
- `http_host`: Your WordPress domain name.
- `http_conf`: Your Apache config file name.
- `http_port`: Your VirtualHost HTTP port (the default port is 80)

## Running the playbook:

- Clone the git repository
```shell
git clone https://github.com/georgi-nachev/ansible-wordpress.git
``` 
- Run the playbook
```shell
ansible-playbook playbook.yml
```
