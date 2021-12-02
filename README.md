# WordPress with MySQL on a separate node on Ubuntu 18.04/20.04

The playbook is designed with one ansible control node and two hosts in mind (one for the Wordpress + LAMP and another for the MySQL DB). Also for the purpose of this test, you need to add an additional firewall rule during your AMI creation, that would allow all "All traffic".

## Obtain the repository

- Clone the git repository
```shell
git clone https://github.com/georgi-nachev/ansible-wordpress.git
cd ansible-wordpress/
``` 

## Playbook Structure

- This is how the `ansible-wordpress` playbook is structured:

```
ansible-wordpress
├── files
│   ├── apache.conf.j2
│   ├── mysqld.cnf.j2
│   ├── wp-config.php.j2
│   ├── table_optimize_script.sh.j2
│   ├── tablesize_before.sql.j2
│   └── tablesize_after.sql.j2
│
├── vars
│   └── default.yml
├── playbook.yml
├── customHosts
└── readme.md
```
- `files/`: directory containing templates and custom scripts required by the playbook.
- `vars/`: directory to save variable files. A `default.yml` var file is included by default.
- `playbook.yml`: the playbook file.
- `customHosts`: the playbook's custom hosts file.
- `readme.md`: instructions and links related to this playbook.

## Considering you have freshly provisioned AWS nodes:

- Install latest updates and upgrades on your ansible control node and hosts
```shell
sudo apt update -y
```
- Install Ansible on your designated control node
```shell
sudo apt install ansible -y
```
- Add a default interpreter. Paste `interpreter_python = /usr/bin/python3` under `[defaults]` section in the `/etc/ansible/ansible.cfg`
```shell
sudo vim /etc/ansible/ansible.cfg
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

- Within the `ansible-wordpress` playbook folder
```shell
vim customHosts
```

- Use the following command to retrieve your IP's
```shell
ip address show
```

### Test your connection

- Test the connection between control node and your hosts. Run the below command while within the playbook folder
```shell
ansible -m ping -i customHosts ,
```

## Variables:

- Final step before you run the playbook is adjusting the variables in `vars/default.yml` to your individual requirements. Read the description below so you can understand better their purpose in this playbook.

- `php_extensions`:  All necessary php extensions for a clean WordPress installation. No need to change this variable, but you might want to adapt it to your needs by extending the list of modules.  
- `database_name`: Your WordPress database name.
- `database_user`: Your WordPress database username.
- `database_user_password`: The password for your WordPress database username.
- `database_ip`: The IP Address of your Database server (as in the `customHosts` file)
- `wordpress_ip`: The IP Address of your WordPress server (as in the `customHosts` file)
- `http_host`: Your WordPress domain name.
- `http_conf`: Your Apache config file name.
- `http_port`: Your VirtualHost HTTP port (the default port is 80)

## Running the playbook:

- Run the playbook
```shell
ansible-playbook playbook.yml -i customHosts
```
