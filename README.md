# Prerequisites
## Installing Vagrant

**Vagrant** is a tool for building and managing virtual machine environments in a single workflow. 
Following the instructions [here](https://www.vagrantup.com/docs/installation/) in order to install it. 

## VirtualBox Provider

[Download](https://www.virtualbox.org/wiki/Downloads) and install Virtual Box as virtualization provider.

# Topology

![image](https://user-images.githubusercontent.com/32895268/111208566-d2b29280-85c2-11eb-9442-038602abaaf6.png)

| Hostname      | Role          
| :------------:|:------------------------:|
| CONTROL       | Configuration management |
| LB01          | Load Balancer            |
| DB01          | Database                 |
| APP01         | Webserver                |
| APP02         | Webserver                |

# Creating VM's

1. [Clone](https://github.com/macedojleo/loadbalancer.git) this project
2. Access the project directory
3. Using the system CLI, run the following Vagrant commands in order to create and provisioning the Virtual Machines.
 
 ```$ vagrant up --provisioning ```

 3.1. Access the **control** VM (where Ansible was installed):
  
 ```$ vagrant ssh control```
 
 3.2. Since you've got access to **control** VM, validate Ansible has been installed successfully.


 ```vagrant@control:~$ ansible --version```

 3.3. Now, from the **control** VM you can have access to another machines by using ssh command.


 ```vagrant@control:~$ ssh lb01```
 
 ```vagrant@control:~$ ssh db01```
 
 ```vagrant@control:~$ ssh app01```
 
 ```vagrant@control:~$ ssh app02```
 
# Setting up Virtual Machines 

Run all following commands in **control** Machine.

1. Pre-install (**ALL MACHINES**)

 ```vagrant@control:~$ ansible-playbook -i /etc/ansible/inventories/hosts /etc/ansible/playbooks/pre-install.yml```

2. Load Balance (**LB01**)

 ```vagrant@control:~$ ansible-playbook -i /etc/ansible/inventories/hosts /etc/ansible/playbooks/nginx.yml```

3. Apache services (**APP01, APP02**)

 ```vagrant@control:~$ ansible-playbook -i /etc/ansible/inventories/hosts /etc/ansible/playbooks/apache2.yml```

4. Data Base (**DB01**)


 ```vagrant@control:~$ ansible-playbook -i /etc/ansible/inventories/hosts /etc/ansible/playbooks/mysql.yml```

# Deploying simple static web page

Run the following command in **control** machine.

 ```vagrant@control:~$ ansible-playbook -i /etc/ansible/inventories/hosts /etc/ansible/playbooks/app.yml```

# Testing load balancer is working property

From web browser access: http://192.168.135.101/page.html

![image](https://user-images.githubusercontent.com/32895268/111078454-6023b300-84ed-11eb-8dc8-ee8a1edbcff7.png)

Reload the page and check if the HTTP traffic was redirected to another web server by NGINX.

![image](https://user-images.githubusercontent.com/32895268/111078490-995c2300-84ed-11eb-8e31-b3ee184638f5.png)

