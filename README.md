# JTorres-Project1
Project1 week13
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/JohnnyT1270/JTorres-Project1/blob/main/Diagrams/Project1NetworkDiagram.PNG

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the  file may be used to install only certain pieces of it, such as Filebeat.

  - filebeat-playbook.yml  ,  filebeat-config.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting high traffic to the network.
What aspect of security do load balancers protect? 
- Load balancers protect servers from overwhemlming traffic, by distributing traffic through multiple servers and
eliminating single servers from being DDOS'D, or other traffic attacks. This will help optimize servers chances of crashing
and utilize maximum productivity!


What is the advantage of a jump box?_
- First, id like to consider a jump box to be a secure location middleman for a transaction. It's a main server that admins
first connect to before launching any administrative commands or tasks, or it's a starting point before connecting to
other servers or untrusted locations.  It will reduce your risk as an organization overall, and wouldn't make sense to not use it!



Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the _____ and system _____.
What does Filebeat watch for?
- Filebeat monitors your log files and forwards them to Elasticsearch or Logstash for indexing

 What does Metricbeat record?_
- Metricbeat records metrics and statistics data then transports them to your output through Elasticserach or Logstash.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name      | Function | IP Address                              | Operating System |
|-----------|----------|-----------------------------------------|------------------|
| Jump Box  | Gateway  | 10.0.0.4(private) 20.120.80.133(public) | Linux            |
| ELK[JTVM] | Server   | 10.1.0.4(private) 40.122.73.192(public) | Linux            |
| Web-1     | Server   | 10.0.0.5(private) 20.124.30.241(public) | Linux            |
| Web-2     | Server   | 10.0.0.6(private) 20.124.30.241(public) | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 74.141.105.74 (localhost ip)

Machines within the network can only be accessed by JumpBox
 Which Machine did you allow access to your ELK VM?
- JumpBox

What was its ip address?
-10.0.0.4

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 74.141.105.74        |
| ELK-JTVM | No                  | 10.0.0.4             |
| Web-1    | No                  | 10.0.0.4             |
| Web-2    | No                  | 10.0.0.4             |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
 What is the main advantage of automating configuration with Ansible?_
- Flexible and very easy to orchestrate the entire application environment no matter where it's deployed, and you can
customize it based on your needs! 

The playbook implements the following tasks:
-In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- First step is to SSH into your Jump-Box (ssh RedAdmin@20.120.80.133)
- Second step is to find your docker container name, and start it and attach it. (sudo docker ps / sudo docker start dreamy_bohr / 
sudo docker attach dreamy_bohr , then you should be in root!
- Now navigate to your /etc/ansible/roles folder, and create your playbook (Elk_Playbook.yml)
- Run the playbook using the command Ansible-Playbook ( Ansible-Playbook Elk_playbook.yml) or Ansible-Playbook /etc/ansible/roles/Elk_playbook.yml
- Last thing to do is verify your elk server is up and running, SSH into the ELK VM, if you connect successfully then you're good to go!


The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/JohnnyT1270/JTorres-Project1/blob/main/Images/docker_ps_output.PNG

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- _TODO: List the IP addresses of the machines you are monitoring_

We have installed the following Beats on these machines:
Specify which Beats you successfully installed_
-metricbeat-7.6.1-darwin-x86_64
-filebeat-7.6.1-darwin-x86_64

These Beats allow us to collect the following information from each machine:
In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
- Metricbeat collects machine metrics to detech how healthy it is, you'll see CPU USAGE, How long the server has been up, etc.
- Filebeat collects log files from your Machines, these files can be generated by Microsoft Azure tools, Nginx WebServer, MySQL databases, Apache, etc.


### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-config.yml file to /etc/ansible/roles/files
- Update the filebeat-config.yml file to include 10.1.0.4 (private ELK VM ip address)
- Run the playbook, and navigate to http://40.122.73.192:5601/ to check that the installation worked as expected.

 Answer the following questions to fill in the blanks:_
- _Which file is the playbook? filebeat-playbook.yml 

- Where do you copy it?_ /etc/ansible/roles

- _Which file do you update to make Ansible run the playbook on a specific machine? hosts file in /etc/ansible/hosts

-How do I specify which machine to install the ELK server on versus which to install Filebeat on?_ You will have groups on the hosts file, webservers and you make a seperate group for ELK servers. Elk server is the ip for the VM that you install elk on, the webservers will have filebeat.

- _Which URL do you navigate to in order to check that the ELK server is running? http://40.122.73.192:5601/

_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._

curl -L -0 https://artifacts.elastic.co/downloads/beats/filebeat/filebeat-7.6.1-darwin-x86_64.tar.gz - to download filebeat
touch filebeat-playbook.yml - to create the playbook file
nano filebeat-playbook.yml - to edit the playbook
ansible-playbook filebeat-playbook.yml - to run the playbook (must be in the directory or specify where it is)
such as( ansible-playbook /etc/ansible/roles/filebeat-playbook.yml )

