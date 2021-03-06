# supreme-garbanzo

## Automated ELK Stack Deployment

*The files in this repository were used to configure the network depicted below.

![Azure Cloud Network](https://github.com/Mjoyner50/supreme-garbanzo/blob/main/diagrams/Marcus%20Resource%20Group.png)

*These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the Azure Cloud Network Diagram file may be used to install only certain pieces of it, such as Filebeat.

   [filebeat-playbook.yml](https://github.com/Mjoyner50/supreme-garbanzo/blob/main/ansible/filebeat-playbook.yml) 
   
   [filebeat-configuration.yml](https://github.com/Mjoyner50/supreme-garbanzo/blob/main/ansible/filebeat-configuration.yml) 

*This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

*The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

- Load balancing ensures that the application will be highly functional, in addition to restricting high-traffic to the network.

- What aspect of security do load balancers protect? 

	- It helps the servers balance traffic and stops the servers from overloading. 
	- It also reroute live traffic from one server to another helping to eliminate single points of failure from attacks such as DDoS attack.

- What is the advantage of a jump box?

	-Jump-box are highly secured computers that are never used for non-admin tasks. 
	-Throughout the years, jump-box has improved into an even more comprehensive/lock-down secure admin workstation to decrease the chances of hackers/malware infection. 


*Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system logs.

- What does Filebeat watch for?
	- It monitors the log files/locations that you specify and forwards them to Elasticsearch/Logstash for indexing.
 
- What does Metricbeat record?
	- It records metrics/statistics data and transports them to the output that you specifics thru Elasticsearch/Logstash.


*The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name                 | Function | IP Address                               | Operating System     |
|----------------------|----------|------------------------------------------|----------------------|
| Jump-Box-Provisioner | Gateway  | 10.0.0.5(Private)//20.102.53.201(Public) |     Linux            |
| ELK-VM               | Server   | 10.2.0.4(Private)//40.112.181.143(Public)|     Linux            |
| Web-1                | Server   | 10.0.0.8(Private)                        |     Linux            |
| Web-2                | Server   | 10.0.0.9(Private)                        |     Linux            |

### Access Policies

*The machines on the internal network are not exposed to the public Internet. 

- Only the jump-Box-Provisioner machine can accept connections from the Internet.
- Access to this machine is only allowed from the following IP addresses:
	- (76.193.xx.xxx)

*Machines within the network can only be accessed by Jump-Box-Provisioner

- Which machine did you allow to access your ELK VM?
	- Jump-Box-Provisioner  

- What was its IP address?
	- 10.0.0.5 (Private) 

- A summary of the access policies in place can be found in the table below.

| Name                  | Publicly Accessible | Allowed IP Addresses |
|-----------------------|---------------------|----------------------|
| Jump-Box-Provisioner  |       Yes           | 20.102.53.201/10.0.0.5|
| ELK-VM*               |       No            | 10.2.0.4             |
| Web-1*                |       No            | 10.0.0.8             |
| Web-2*                |       No            | 10.0.0.9             |

* --All these VMs can only be accessed form the Jump-Box-Provisioner--

### Elk Configuration

*Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- What is the main advantage of automating configuration with Ansible?

	- One main advantage would be YAML Playbooks. It is the best alternative for configuration management/automation.
	- It is also able to automate complex multi-tier IT application environemtns.     
                               
*The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._

	- First, SSH into the Jump-Box-Provisioner (ssh mjoyner@20.102.53.201)
	- Find name of ansible container: "sudo su", "docker container ls -a" (example name:relaxed_ganguly)
	- Start/Attached to the ansible docker: "sudo su", "docker start relaxed_ganguly", "docker attach relaxed_ganguly"
	- Went to /etc/ansible directory and created the ELK playbook (elk1.yml)
	- Ran the elk1.yml in that same directory (ansible-playbook elk1.yml)
	- Lastly, SSH into the ELK-VM to verify the server is up and running.

*The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[ELK-VM "Docker PS"](https://github.com/Mjoyner50/supreme-garbanzo/blob/main/linux/Elk%20Server.PNG)

### Target Machines & Beats
*This ELK server is configured to monitor the following machines:

- List the IP addresses of the machines you are monitoring

	- Web-1 (10.0.0.8)
	- Web-2 (10.0.0.9)
	

- We have installed the following Beats on these machines:

   - Metricbeat
   - Filebeat


*These Beats allow us to collect the following information from each machine:
- In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
	
	- Filebeat is used to collect log files from specific files on remote machines.
	- Examples of Filebeats can be files that are generated by Apache, Microsoft Azure tools, the Nginx web server, and MySQl databases.	

	- Metricbeat collects machine metrics.
	- It is simply a measurement to tell analysts how healthy it is.
	- Examples of Metricbeat can be CPU usage/Uptime 

### Using the Playbook
*In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:

---ELK---

- Copy the elk1.yml file to /etc/ansible
- Update the hosts file to include a "webservers" section and an "elk" section.
- Run the playbook (ansible-playbook elk1.yml), ssh into ELK-VM and run "docker ps" to verify installation worked as expected.

_Answer the following questions to fill in the blanks:_

- Which file is the playbook? 
   - elk1.yml
- Where do you copy it? 
   - /etc/ansible
- Which file do you update to make Ansible run the playbook on a specific machine? 
   - /etc/ansible/hosts file. 
- How do I specify which machine to install the ELK server on versus which to install Filebeat on? 
   - There are two separate groups in the etc/ansible/hosts file. One is for servers, web 1 & 2, which has the IPs of the VMs that will have Filebeat on it. The other group is the elk server, which will have the IP of the VM that has ELK install on it.
- Which URL do you navigate to in order to check that the ELK server is running? 
   - http://40.112.181.143:5601/ 


