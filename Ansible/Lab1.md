# Getting Started with Ansible
## Intro
I'm using [Ansible for Devops][ab140a78]  to get started with Ansible. This book was recommended by Amaozon "Patrik J. Kerpan" as a good book to learn by doing. He further stated that each of the example provide value on how you use this framework for real world purposes.

In this lab I will get started with Ansile by first orchestrate some AWS EC2 server. Then I will use vagrant to start up a Centos 7 server.
## Overview
### What is Ansible
- Simple automation languange that describe an IT application infrastructure.
- Automation engine that runs Ansible playbooks
- Ansible tower is an enterprise framework for controlling, securing, and managing your Ansible automation with a UI and restful API.

![](images/2018/03/Lab1_ansible_arch.png) | ![](images/2018/03/Lab1_ansible_can_do.png)
-------------------------|-------------------------
Ansible Architecture | What can Ansible Do  
## Details
### Getting Started with Ansible
I am using Ubuntu 16.04. I first started by installing Ansible with this commands.

`sudo  apt-add-repository -y ppa:ansible/ansible`

`sudo apt-get update`

`sudo apt-get install -y ansible `

Now let's use Ansible to connect to an AWS server. I had already created an EC2 instance therefore I will not showcase that process. But In order to get access to my EC2 through SSH I had to go through some loophole.

1. First I had to create a ssh rsa key pair on my local machine `ssh-keygen -t rsa` this created “/.ssh/id_rsa" and "/.ssh/id_rsa.pub"

2. Now login on the aws server through ssh I was able to tun the following commands.`useradd -m <yourname> ` `sudo su <yourname>` `cd ` `mkdir -p ~/.ssh` `touch ~/.ssh/authorized_keys` `chmod -R 700 ~/.ssh` `chmod 600 ~/.ssh/*`
3. The I uploaded my local ~/.ssh/id_rsa.pub to the server and passed it to the new authorized_keys files that I created `cat id_rsa.pub  > ~/.ssh/authorized_keys`
4. Login for Ansible was accepted after that.

Running Ansible command came with some issue because of AWS using python3 command to run python program. We need to tell Ansible to use python3 as the default python. Configuration of the ansible server is saved under /etc/ansible/hosts. This host file keep an inventory of the servers that Ansible can use. In order to use python3 the following needed to be done.

```shell
$: Vagrant box add geerlingguy/centos7
$: Vagrant init geerlingguy/centos7
$: Vagrant up
```

```yaml
---
- hosts: all
become: yes
tasks:
- name: Ensure NTP (for time synchronization) is installed.
  yum: name=ntp state=present
- name: Ensure NTP is running.
  service: name=ntpd state=started enabled=yes 		
...
```


## Conclusion

## References

---

  [ab140a78]: https://www.amazon.com/Ansible-DevOps-Server-configuration-management/dp/098639341X/ref=sr_1_1?ie=UTF8&qid=1521573918&sr=8-1&keywords=ansible+for+devops "Ansible for DevOps"
