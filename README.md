# Virtual-Network-Project-1
Using Microsoft Azure I created a small stable virtual network. This network includes a Jump Box acting as the ansible control node, two DVWA virtual machines to act as web servers, and an ELK server configured with Filebeat to monitor DVWA-VM1/2.

## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

[/Users/nickyneu/Virtual-Network-Project-1/Diagram/vnet-diagram.png]

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the playbook file may be used to install only certain pieces of it, such as Filebeat.

Enter the playbook file:
           filebeat-playbook.yml

This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly stable, in addition to restricting traffic to the network.

What aspect of security do load balancers protect? 

-Load balancers' primary function it so spread workloads across multiple servers within a layered security model. Load balancers prevent overloading servers and optimzie productivity. Additionally, they add resiliency to a network by rerouting traffic between servers if one server should fall prey to an attack. Because load balancers have health probes they are able to make sure a server is running properly before sending traffic to it, if a server is not running properly due to a DDoS attack, the load balancer will send the traffic to a functioning server. This helps to reduce the attack surface, eliminate single points of failure, and makes it more difficult for attackers to exhaust a network's resources. 

What is the advantage of a jump box?

-A jump box is very similar to a gateway router. The jump box is exposed to the public internet and can be accessed via port 22 though an SSH connection. The jump box sits in front of other machines that are not exposed to the public internet, and is able to control access to these machines by allowing connections from specific IP addresses to them. This is a way that the jump box helps improve security.


Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the network and system configuration.

What does Filebeat watch for?

-Filebeat logs information about the file system and sends this information to Logstach and Elasticsearch. Filebeat is used to collect data about specific files and changes made to them on remote machines. 

What does Metricbeat record?

-Metricbeat can be installed on servers to routinely collect metrics from the operating system and services running on the server. Metricbeat then takes the stats and metrics it gathered, and sends them to an output like Elasticsearch or Logstach. 

The configuration details of each machine may be found below.

| Name                 | Function  | IP Address | Operating System | Virtual Network | Location  | Availability Zone | Exposed Ports | IP Addresses Allowed    | Containers Installed  |
|----------------------|-----------|------------|------------------|-----------------|-----------|-------------------|---------------|-------------------------|-----------------------|
| Jump-Box-Provisioner | Gateway   | 10.0.0.1   | Linux V1         | RedNet          | East US   | 1                 | 22, 80        | 99.159.25.172, 10.0.0.4 | Ansible controle node |
| DVWA-VM1             | Webserver | 10.0.0.5   | Linux V1         | RedNet          | East US   | 1                 | 22, 80        | 99.159.25.172, 10.0.0.4 | Ansible               |
| DVWA-VM2             | Webserver | 10.0.0.6   | Linux V1         | RedNet          | East US   | 1                 | 22, 80        | 99.159.25.172, 10.0.0.4 | Ansible               |
| ELK                  | Webserver | 10.1.0.4   | Linux V1         | Red-Team-vnet   | East US 2 | 2                 | 22, 80, 5601  | 99.159.25.172, 10.0.0.4 | ELK                   |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- 99.159.25.172

Machines within the network can only be accessed by ssh connection through the ansible container.

Which machine did you allow to access your ELK VM? What was its IP address?

-My ELK VM can be accessed after SSH into the Jump-Box-Provisioner VM and then opening up and attaching to the ansible container. From the ansible container, you can then SSH into the ELK VM using the IP address 10.1.0.4. The IP address of the Jump-Box-Provisioner is 10.0.0.4. 

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible | Allowed IP Addresses    |
|----------------------|---------------------|-------------------------|
| Jump-Box-Provisioner | No                  | 99.159.25.172           |
| DVWA-VM1             | No                  | 10.0.0.4, 99.159.25.172 |
| DVWA-VM2             | No                  | 10.0.0.4, 99.159.25.172 |
| ELK                  | No                  | 10.0.0.4, 99.159.25.172 |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...

What is the main advantage of automating configuration with Ansible?

-One of the main advantages of automating configurations with Ansible is that Ansible uses a simple syntax written in YAML called playbooks. These playbooks can be written to automate storage, servers, and networking. These manual tasks can be difficult to repeat, so using Ansible to configure these components makes the tasks easier to complete and less vulnerable to error. 

The playbook implements the following tasks:

In 3-5 bullets, explain the steps of the ELK installation play:
   - sudo apt-get update && apt-get upgrade
   - sudo apt-get dockier.io
   - sudo apt-get install docker.io
   - sudo systemctl status docker
   - sudo docker pull sebp/elk
   - sudo docker run -it sebp/elk
   - sudo docker start elk
   - sudo docker attach elk

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

[/Users/nickyneu/Virtual-Network-Project-1/Docker-Image/docker ps.jpg]

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- List the IP addresses of the machines you are monitoring

      10.0.0.5
      10.0.0.6

We have installed the following Beats on these machines:
    -Filebeat

These Beats allow us to collect the following information from each machine:

-Filebeat is an agent installed on your server that monitors the log files and their specific locations, collects log events, and forwards this information to Elasticsearch or Logstash.
-system.log is an example of something I would expect to see filebeats collecting.

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the filebeat-configuration.yml file to the /etc/ansible/files directory.
- Update the filebeat-playbook.yml file to include the source path of the filebeat-configuration.yml file.
- Run the playbook, and navigate to 20.186.59.100:5601 to check that the installation worked as expected.

The following screen shot displays the results of filebeat correctly installed on the ELK server. This displays the Kibana site with graphs monitoring the 2 DVWA-VMs. 

[/Users/nickyneu/Virtual-Network-Project-1/Filebeat-Image/filebeat-graphs.png]
