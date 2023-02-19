# ansibleserver
Ansible is a configuration management, deployment & ochestration tool
ansible: Application deployment
Configuration management
Security & Compliance
Orchestration
Provisioning

Application deployment: APP ---> Git -----> Ansible ----> Jenkins (optional) ---Servers (i.e. test server or prod server)
APP ---> Git -----> jenkins ----> Servers (i.e. test server or prod server)

Configuration management:

Security & Compliance: (Define security wwith playbooks, writes ansible roles to apply DISA STIG, works with existing SSH
and WinRM protocols, Compatible with different tools for security verification)

Orchestration: orchestrate the order of process and required installations

ANSIBLE IS A PUSH ARCHITECTURE, Puppet and Chef are PULL


Installing Ansible (first option? centos)
#!/bin/bash (as a script) - check chuck youtube (you need to learn ansible right now!!)
$ yum update -y
$ yum install epel-release -y
$ yum install ansible -y

yum - centOS
apt-get - ubuntu
These should be noted in playbooks also


Installing Ansible: (second option? - ubuntu)
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository ppa:ansible/ansible
$ sudo apt-get update
$ sudo apt-get install ansible

To see the version of ansible installed
$ ansible --version

Host inventory:
[webservers] server1
[application] server1 server2
default path: /etc/ansible/hosts
on terminal can be access using: sudo nano /etc/ansible/hosts


YAML file starts with ... and ends with ...:
...
james:
name: james john
rollNo: 34
div: B
sex: male
...


    And can be rewritten:
      james: {name: james john, rollNo: 34, div: B, sex: male}


      for list
countries:
- America
- China
- Canada
- Iceland

      can be rwritten
countries: {'America', 'China', 'Canada', 'Iceland'}



Assessing the host file (where to name servers and group according to IP add or Dns)
$ cd /etc/ansible
$ sudo vi hosts



In Ansible, the '*' character is used as a wildcard and can have different meanings depending on the context.

In a playbook, if * is used in the hosts: field, it means that the playbook will be executed on all hosts defined in the inventory file. For example, if your inventory file defines the group webservers with several hosts, then hosts: webservers will target only those hosts, whereas hosts: * will target all hosts defined in the inventory file regardless of the group they belong to.

In a task, if * is used in a module's parameter, it means that the module will apply to all items in a list. For example, in a file module, state: directory will create a directory, whereas state: file will create a file. If you want to create both, use state: directory, file

In a task, if * is used in a loop, it means that the task will be executed on all items in the list. For example, in a yum task, name: '*' will update all packages, whereas name: 'httpd' will only update the httpd package.

It's important to note that the wildcard can have different meanings depending on the context, so make sure you understand how it is being used in your playbook.


##Here the variables can be assessed by all the tasks in the playbook:
- name: Demo on vars play level
  hosts: localhost
  vars:
  URL: play.example.com
  tasks:
    - name: Print URL
      ansible.builtin.debug:
      msg: URL = {{ URL }}

##Here only the play in the tasks can asses the variables
- name: Demo on vars on task level
  hosts: localhost
  tasks:
    - name: Print URL
      ansible.builtin.debug:
      msg: URL {{ URL }}
      vars:
      URL: task.example.com

##Here to assess variables from a file to a playbook
- name: Demo on vars from file on play level
  hosts: localhost
  vars_files:
    - sample-vars.yml
      tasks:
    - name: Print URL
      ansible.builtin.debug:
      msg: URL {{ URL }}

VARIABLE PRECEDENCE WHILE ROLES ARE USED:
Command line variables
Task level variable
Vars dir from roles
Variable from files
Play level variable
Inventory variable
defaults dir from roles



NOTES: 
BOOL1: true
STRING: "True"  #If you double quote a boolean it becomes strings

#if you double quote a NUMBER, it is still a number
NUMBER: 7
NUMBER: "7"


In ansible quotes are if the value is starting with a variable
i.e. msg: "{{UPTIME}}"
but not necessary here:
i.e. msg: URL = {{ URL }} #because it didnt start with a variable in the line


From the ansible server, you can install apps in the host server. This is great when you have many 
nodes/IP address. syntax below
# ansible linux -m yum -a "name=vim state=present" - on centos
# ansible linux -m apt -a "name=nano state=present" - on ubuntu where linux is the name of
the grouped IP address