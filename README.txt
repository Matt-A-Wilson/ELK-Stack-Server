## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://github.com/Matt-A-Wilson/ELK-Stack-Server/tree/e008f9e8a40beb495ba4b9fe57cae2c093e6ba3a

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the install-elk.yml file may be used to install only certain pieces of it, such as Filebeat.

 ---
- name: Config Web VM with Docker
  hosts: webservers
  become: true
  tasks:
    - name: docker.io
      apt:
        update_cache: yes
        name: docker.io
        state: present

    - name: Install pip3
      apt:
        name: python3-pip
        state: present

    - name: Install Docker python module
      pip:
        name: docker
        state: present

    - name: download and launch a docker web container
      docker_container:
        name: dvwa
        image: cyberxsecurity/dvwa
        state: started
        restart_policy: always
        published_ports: 80:80

    - name: Enable docker service
      systemd:
        name: docker
        enabled: yes


This document contains the following details:
- Description of the Topology
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly accessible, in addition to restricting traffic to the network.
Load balancers further defend against such attacks as DDoS. A jump bx offers an addded level of security by providing a secure environment for admins to operate in. 

Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the files and system metrics.
Filebeat monitors log files and Metricbeat creates output for Elasticsearch. 

The configuration details of each machine may be found below.

| Name                 | Function         | IP Address | Operating System |
|----------------------|------------------|------------|------------------|
| Jump Box Provisioner | Gateway          | 10.0.0.4   | Linux            |
| Web-1                | VM/DVWA 1/Docker | 10.0.0.5   | Linux            |
| Web-2                | VM/DVWA 2/Docker | 10.0.0.6   | Linux            |
| ELK                  | Network Security | 10.1.0.7   | Linux            |

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the Jump Box Provisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
73.24.182.2

Machines within the network can only be accessed by the Jump Box Provisioner.

A summary of the access policies in place can be found in the table below.

| Name                 | Publicly Accessible  | Allowed IP Addresses |
|----------------------|----------------------|----------------------|
| Jump Box Provisioner | Yes                  | 73.24.182.2          |
| Web-1                | No                   | 10.0.0.5/10.0.0.6    |
| Web-2                | No                   | 10.0.0.5/10.0.0.6    |
| ELK Server           | No                   | 73.24.182.2          |

### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because automation maintanes consistency throughout its use. 

The playbook implements the following tasks:
- Install Docker.io
- Install python3-pip3
- Install Docker module
- Download and launch an ELK Container

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

https://github.com/Matt-A-Wilson/ELK-Stack-Server/blob/main/docker_ps_output.PNG

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
- 10.0.0.5
- 10.0.0.6

We have installed the following Beats on these machines:
- Filebeat
- Metricbeat

These Beats allow us to collect the following information from each machine:
Filebeat monitors the log files, collects log event data and sends that information to the ELK server. From the ELK server, the information is compiled for viewing and indexing. Metricbeat collects metrics from the operating systems of Web-1 and Web-2 and provides that information to the ELK server for indexing.   

### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the install-elk.yml file to /etc/ansible.
- Update the host file to include the IP Address on the ELK server.
- Run the playbook, and navigate to https://23.96.99.53:5601/app/kibana to check that the installation worked as expected.
