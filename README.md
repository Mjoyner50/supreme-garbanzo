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

	- It helps prevent overloading servers as well as optimizes productivity and maximizes uptime. 
	- It also adds resiliency by rerouting live traffic from one server to another causing it to eliminate single points of failure from attacks such as DDoS attack.

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
| Jump-Box-Provisioner | Gateway  | 10.0.0.4(Private)//(Public) |     Linux            |
| ELK-VM               | Server   | 10.1.0.4(Private)//(Public) |     Linux            |
| Web-1                | Server   | 10.0.0.9(Private)                        |     Linux            |
| Web-2                | Server   | 10.0.0.10(Private)                       |     Linux            |
| Web-3                | Server   | 10.0.0.11(Private)                       |     Linux            |

### Access Policies

*The machines on the internal network are not exposed to the public Internet. 

- Only the jump-Box-Provisioner machine can accept connections from the Internet.
- Access to this machine is only allowed from the following IP addresses:
	- (LocalHost IP address)

*Machines within the network can only be accessed by Jump-Box-Provisioner

- Which machine did you allow to access your ELK VM?
	- Jump-Box-Provisioner  

- What was its IP address?
	- 10.0.0.8 (Private) 

- A summary of the access policies in place can be found in the table below.

| Name                  | Publicly Accessible | Allowed IP Addresses |
|-----------------------|---------------------|----------------------|
| Jump-Box-Provisioner  |       Yes           | Localhost Public IPV4|
| ELK-VM*               |       No            | 10.0.0.8             |
| Web-1*                |       No            | 10.0.0.9             |
| Web-2*                |       No            | 10.0.0.10            |
| Web-3*                |       No            | 10.0.0.11            |

* --All these VMs can only be accessed form the Jump-Box-Provisioner--

### Elk Configuration

*Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

- What is the main advantage of automating configuration with Ansible?

	- One main advantage would be YAML Playbooks. It is the best alternative for configuration management/automation.
	- It is also able to automate complex multi-tier IT application environemtns.     
                               
*The playbook implements the following tasks:
- In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._

	- First, SSH into the Jump-Box-Provisioner (ssh admin@52.170.129.21)
	- Find name of ansible container: "sudo su", "docker container ls -a" (example name: zealous_yalow)
	- Start/Attached to the ansible docker: "sudo su", "docker start zealous_yalow", "docker attach zealous_yalow"
	- Went to /etc/ansible directory and created the ELK playbook (install-elk.yml)
	- Ran the install-elk.yml in that same directory (ansible-playbook install-elk.yml)
	- Lastly, SSH into the ELK-VM to verify the server is up and running.

*The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[ELK-VM "Docker PS"]()

### Target Machines & Beats
*This ELK server is configured to monitor the following machines:

- List the IP addresses of the machines you are monitoring

	- Web-1 (10.0.0.9)
	- Web-2 (10.0.0.10)
	- Web-3 (10.0.0.11)

- We have installed the following Beats on these machines:

	- [Filebeat Module Status Screenshot])

1. Create filebeat-config.yml: nano filebeat-config.yml, paste text from provided file and update with correct ip addresses.
2. Create filebeat-playbook.yml: nano filebeat-playbook, paste text from provided file. Verify correct "src" for absoulte path to filebeat-config.yml.
3. Run playbook: "ansible-playbook filebeat-playbook.yml"

	- [Metricbeat Module Status Screenshot]()

1. Create metricbeat-config.yml: nano metricbeat-config.yml, paste text from provided file and update with correct ip addresses.
2. Create metricbeat-playbook.yml: nano metricbeat-playbook, paste text from provided file. Verify correct "src" for absolute path to metricbeat-config.yml.
3. Run playbook: "ansible-playbook metricbeat-playbook.yml"

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

- Copy the install-elk.yml file to /etc/ansible
- Update the Ansible-hosts file to include a "webservers" section and an "elk" section.
- Run the playbook (ansible-playbook install-elk.yml), ssh into ELK-VM and run "docker ps" to verify installation worked as expected.

_Answer the following questions to fill in the blanks:_

- Which file is the playbook? 
   - install-elk.yml
- Where do you copy it? 
   - /etc/ansible
- Which file do you update to make Ansible run the playbook on a specific machine? 
   - /etc/ansible/hosts file (IP of the Virtual Machines). 
- How do I specify which machine to install the ELK server on versus which to install Filebeat on? 
   - I have to specify two separate groups in the etc/ansible/hosts file. One of the groups will be webservers which has the IPs of the VMs that I will install Filebeat to. The other group is named elkservers which will have the IP of the VM I will install ELK to.
- Which URL do you navigate to in order to check that the ELK server is running? 
   - http://IPV4:5601/ 


