# Installing Vagrant

**Vagrant** is a tool for building and managing virtual machine environments in a single workflow. 
Following the instructions [here](https://www.vagrantup.com/docs/installation/) in order to install it. 

# VirtualBox Provider

[Download](https://www.virtualbox.org/wiki/Downloads) and install Virtual Box as virtualization provider.

# Creating VM's

1. [Clone](https://github.com/macedojleo/loadbalancer.git) this project
2. Access the project directory
3. Using the system CLI, create and provisioning the Virtual Machines.
 
 ```$ vagrant up --provosioning ```

 4. Access the **control** VM (where Ansible was installed):
  
 ```$ vagrant ssh control```
 
 5. Since you've got access to **control** VM, validate Ansible has been installed successfully.


 ```vagrant@control:~$ ansible --version```

 6. You can have access from **control** VM to another machines by using ssh command.


 ```vagrant@control:~$ ssh lb01```
 
 ```vagrant@control:~$ ssh db01```
 
 ```vagrant@control:~$ ssh app01```
 
 ```vagrant@control:~$ ssh app02```
 
# Setting up Virtual Machines 

Run all following commands from **control** VM.

1. Pre-install (**ALL MACHINES**)

 ```vagrant@control:~$ ansible-playbook -i /etc/ansible/inventories/hosts /etc/ansible/playbooks/pre-install.yml```

2. Load Balance (**LB01**)

 ```vagrant@control:~$ ansible-playbook -i /etc/ansible/inventories/hosts /etc/ansible/playbooks/nginx.yml```

3. Apache services (**APP01, APP02**)

 ```vagrant@control:~$ ansible-playbook -i /etc/ansible/inventories/hosts /etc/ansible/playbooks/apache2.yml```

4. Data Base (**DB01**)


 ```vagrant@control:~$ ansible-playbook -i /etc/ansible/inventories/hosts /etc/ansible/playbooks/mysql.yml```

# Deploying simple static web page

Run the following command from **control** VM.

 ```vagrant@control:~$ ansible-playbook -i /etc/ansible/inventories/hosts /etc/ansible/playbooks/app.yml```

# Testing load balancer is working property

From web browser access: http://192.168.135.101/page.html

![image](https://user-images.githubusercontent.com/32895268/111078454-6023b300-84ed-11eb-8dc8-ee8a1edbcff7.png)

Reload the page and check if the HTTP traffic was redirected to another web server by NGINX.

![image](https://user-images.githubusercontent.com/32895268/111078490-995c2300-84ed-11eb-8e31-b3ee184638f5.png)

