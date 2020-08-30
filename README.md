# Network-Project-1## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

README/Images/Network_Diagram.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the ansible file may be used to install only certain pieces of it, such as Filebeat.

  - playbook.yaml
  - filebeat-config.yml
  - install-elk.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available and provide redundancy, in addition to restricting certain traffic to the network.
- What aspect of security do load balancers protect? What is the advantage of a jump box?
 - The aspect of security that load balancers protect is from Denial of Service attacks. The advantage of a jump box is that if it acts as a gateway router to the rest of the web containers. By focusing traffic through a single node, it makes it easier to implement routing logic and design networks.

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the logs and system traffic.
- What does Filebeat watch for?
  -Filebeat watches log files and locations, while also collecting log events
- What does Metricbeat record?
 - Metricbeat records the machine metrics, such as uptime and CPU usage.

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function  | IP Address | Operating System    |
|----------|-----------|------------|---------------------|
| Jump Box | Gateway   | 10.0.0.8   | Linux (ubuntu 18.04)|
| Web-1    |Docker-DVWA| 10.0.0.9   | Linux (ubuntu 18.04)|
| Web-2    |Docker-DVWA| 10.0.0.10  | Linux (ubuntu 18.04)|
| Web-3    |Docker-DVWA| 10.0.0.13  | Linux (ubuntu 18.04)|
| ELK-VM   |Docker-DVWA| 10.1.0.4   | Linux (ubuntu 18.04)|

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- Personal IP: 71.162.186.207

Machines within the network can only be accessed by SSH.
-The only machine allowed to access the ELK-VM (10.1.0.4) is the the Jump Box from its private IP (10.0.0.8)

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses                     |
|----------|---------------------|------------------------------------------|
| Jump Box |     No              | Personal IP Address (71.162.186.207      |
| Web-1    |     No              | 10.0.0.8                                 |
| Web-2    |     No              | 10.0.0.8                                 |
| Web-3    |     No              | 10.0.0.8                                 |
| Elk-VM   |     No              | 10.0.0.8 and Personal IP (71.162.186.207)|

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- The main advantage of autamating through Ansible is the ease of use and being able to configure multiple machines with the use of playbooks after initial creating it.

The playbook implements the following tasks:
- Creates a new VM which will be SSH into from the VM and the public IP connected to the Kibana site to view all the metrics and syslogs.
- Downloads and configues an "elk" docker container.
 - In the hosts.conf file in the ansible directory, you will need to nano into it to add a new group [elkserver] and the private IP (10.1.0.4) to the group. 
- Then you will need to create a new ansible-playbook that will download, install, and configure the Elk server to map to the ports 5601, 9200, and 5044.
- Launch the container and you can verify that the container is up and running by SSH into the container from the Jumpbox.
- In the security group for ELK, you will need to create a new Inbound Security Rile to allow ports 5601and 9200 which will allow access from your personal network
- After that is setup, you can open your preferred browser and type in [Public IP:5601] into the browsers which will allow access into the Kibana Portal site if everything is set up correctly. 

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

README/Images/Elk_Docker.PNG

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.8
- 10.0.0.9
- 10.0.0.10
- 10.0.0.13

We have installed the following Beats on these machines:
- Filebeat and Metricbeat

These Beats allow us to collect the following information from each machine:
- Filebeat is a lightweight shipper that helps generate and organizes log files to send to Logstash and Elastic search.
 - It will monitor the log files or locations that you specify, collect the log events to be sent.
- Metricbeat collects metcis from the operating system and from services running on the server.
 - it will take those metrics and statistics and then send them to the output that you specify.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat.yml file to /etc/ansible/roles/files/ directory.
- Update the configuration file to include...
 - The Private IP of the Elk-Server to the ElasticSearch and Kibana sections of the configuration files.
- Run the playbook, and navigate to ELK-VM to check that the installation worked as expected.

TODO: Answer the following questions to fill in the blanks:_
- Which file is the playbook? Where do you copy it?
 - filebeat_config.yml and it should be copied to the /etc/ansible/roles/files/ directory 
- Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
 - The file that is needed to update to make Ansible run the playbook on the specific machine is the filebeat_config.yml which is a config file that will be put into the Elk VM during the run of the ansible-playbook. When you update the host.cfg in the ansible directory, you will need to create a new group called [elkserver]. When configuring the filebeat.yml file, you'll need to designate the Private IP of the Elk-Server.
- Which URL do you navigate to in order to check that the ELK server is running?
 - The URL to use to verify the Elk Vm is running is the [Public IP:5601]
