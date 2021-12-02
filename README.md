# WordPress with MySQL on a separate node on Ubuntu 18.04/20.04

The playbook is designed with one ansible control node and two hosts in mind (one for the Wordpress + LAMP and the another one for the MySQL DB). Also for the purpose of this test, you need to add a firewall rule that would allow all "All traffic" during the AMIs creation process.

## Considering you have freshly provisioned AWS nodes:

- Install latest updates and upgrades on your ansible control node and hosts
```shell
sudo apt update -y
sudo apt upgrade -y
sudo apt install python -y
```
- Install Ansible on your designated control node
```shell
sudo apt install ansible -y
```
- Add a default interpreter. Paste `interpreter_python = /usr/bin/python3` under `[defaults]` section in the `/etc/ansible/ansible.cfg`
```shell
sudo vim ansible.cfg
```

## Set up a connection between your master and child nodes 

- Populate your public key (id_rsa.pub) into your authorized_keys file on your child hosts. You can either use an existing public key, or generate a new one 
 ```shell
 ssh-keygen
 ```
 - Copy the public key to your clipboard
 ```shell
cat ~/.ssh/id_rsa.pub
```
- Put your public key into the `authorized_keys` file on each of your ansible hosts
```shell
vim ~/.ssh/authorized_keys
```

## Adjust your hosts file

- The playbook comes with a custom hosts file. You need to fill in the IP of your wordpress node under the `[wordpress]` section and the IP of your database node under `[database]` section

- Find your IP's using the below command
```shell
ip address show
```

### Test your connection

- Test the connection between control node and your hosts. Run the below command while within the playbook folder
```shell
ansible -m ping -i customHosts ,
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
cd ansible-wordpress/
``` 
- Run the playbook
```shell
ansible-playbook playbook.yml -i customHosts ,
```
