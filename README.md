# CyberSecurity
Course material from Columbia University's School of Engineering CyberSecurity Course
## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/tharshman/CyberSecurity/tree/main/Diagrams/Azure_3region_ELK_Design.png

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook files may be used to install only certain pieces of it, such as Filebeat.

https://github.com/tharshman/CyberSecurity/tree/main/Ansible

NOTE: YAML files were copied off to Mac OSx and in rich text format - reason for .rtf extension
hosts
ansible-config.yml.rtf
ansible-playbook.yml.rtf
filebeat-config.yml.rtf
filebeat-playbook.yml.rtf
metricbeat-config.yml.rtf
metricbeat-playbook.yml.rtf
install-elk.yml.rtf



This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly available, in addition to restricting unwanted access to the network.
- Load balancers increase high availability by placing multiple instances behind a single point of presence for the user. This allows failure of a node without disruption to the service rendered. A load balancer provides a single IP that governs access and can be used to provide failover and higher capacity by spreading the load. There are algorithms in place that facilitate transferring users when a node fails over to a surviving node. The load balancer protects direct access to the node keeping the nodes identity restricted from outside sources.
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system administration.
- Filebeat watches log files and particular events in those logs
- Metricbeat captures and records OS information and provides that information to dsahboards in Elasticsearch

The configuration details of each machine may be found below.

| Name     | Function         | IP Address | Operating System |
|----------|----------        |------------|------------------|
| Jump Box | Gateway          | 10.0.1.4   |Linux Ubuntu 18.04|
| Web-1    | DVWA-Web Server  | 10.0.1.7   |Linux Ubuntu 18.04|
| Web-2    | DVWA-Web Server  | 10.0.1.8   |Linux Ubuntu 18.04|
| Web-3    | DVWA-Web Server  | 10.0.1.5   |Linux Ubuntu 18.04|
| DVWA-VM3 | DVWA-Web Server  | 10.1.0.6   |Linux Ubuntu 18.04|
| DVWA-VM4 | DVWA-Web Server  | 10.1.0.7   |Linux Ubuntu 18.04|
| ELK-vm1  | Elasticsearch    | 10.3.0.4   |Linux Ubuntu 18.04|

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jumpbox and ELK machine can accept connections from the Internet. Access to these machines is only allowed from the following IP addresses:
- 47.201.101.146
This is currently the only allowed machine into the Jumpbox and ELK stack. This is a remote system's public facing gateway inbound from the Internet

Machines within the network can only be accessed by SSH from within Ansible on the Jumpbox provisioner or the Jumpbox directly if SSH key from both systems is loaded into VM
- the Jumpbox @ 10.0.1.4 or currently from outside 47.201.101.146

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses        |
|----------|---------------------|-----------------------------|
| Jump Box | Yes                 | 47.201.101.146              |
| Web-1    | No                  | 10.0.1.4                    |
| Web-2    | No                  | 10.0.1.4                    |
| Web-3    | No                  | 10.0.1.4                    |
| DVWA-VM3 | No                  | 10.0.1.4                    |
| DVWA-VM4 | No                  | 10.0.1.4                    |
| Elk-vm1  | Yes                 | 47.201.101.146 and 10.0.1.4 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because it saves time as systems scale and becomes more difficult and less resourceful to manually address each machine. A single configuration and playbook allows for a central configuration that in conjuction with the hosts file ensures systems are deployed consistently removing more of the possiblity of errors indifferent across a battery of machines. Human error in a manual process adds additional risks that the farm will not be consistent or the possbility of exposure. The playbook allows for quick assessment of a potentially high risk situaiton and favors risk averse.

The playbook implements the following tasks:
- Install docker
- Download the file image for applications
- Make necessary changes to the config files
- Create the playbook
- Configure the ansible hosts file to by adding your ELK vm to the section titled [elk] and add the python interpreter - See attached hosts file in the Ansible directory
- Run the install-elk.yml playbook

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/tharshman/CyberSecurity/tree/main/Images/docker_ps_ELK.png

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.1.7
- 10.0.1.8
- 10.0.1.5
- 10.1.0.7
- 10.1.0.6

We have installed the following Beats on these machines:
- Filebeats
- Metricbeats

These Beats allow us to collect the following information from each machine:
- Filebeat is used to collect log information using an agent on the designated host and recieves output. It then can be leveraged to asses situations, e.g, permissions and access issues or intrusive and bad actor behavior. Can also look at logs for trends on machines that dictate change and configuration changes.
- Metricbeat is used to monitor machine performance and works on the OS of the machine. It can monitor cpu performance, memory performace, disk usage, etc. It can have thresholds set to signal events that may put the system at risk.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the configuration YAML file to /etc/anisble directory.
- Update the YAML file to include the necessary parameters for remote user account, IP addresses for ELK as well as port configurations
- Ensure the hosts files contain the private IP addressed of the machines in their appropriate sections, e.g, [webservers] and [elk]
- Run the playbook, and navigate to web browser and enter the public IP of either your ansible web vms and test with HTTP://<DVWA_IP_Address>/setup.php and directly to ELK machine with HTTP://<ELK_server_IP>:5601/app/kibana to see if your Kibana dashboard comes up checking the installation worked as expected.

### Commands used to run a playbook
First is to connect to the Jumpbox via the public interface provided
- from a terminal on the host machine - ssh <username>@<Public_IP_Jumpbox>
- Once in you will need to attach to Ansible
  - sudo docker ps
This outputs the ansible named container
  - sudo docker start <ansible_container>
  - sudo docker attach <ansible_container>
Now you are in ansible
  - navigate to the container directory - cd /etc/ansible (create if you don't have it)
You need to use curl to access paths to packages and configuration templates
  - curl https://gist.github.com/slape<filename in git format> > /etc/ansible/<filename>
      - metricbeat - https://gist.github.com/slape/58541585cc1886d2e26cd8be557ce04c
      - filebeat - https://gist.github.com/slape/5cc350109583af6cbe577bbcc0710c93
  - nano <filename.yml>
  - ansible-playbook <filename>-playbook.yml
