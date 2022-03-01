## Automated ELK Stack Deployment

The files in this repository were used to configure the network depicted below.

https://drive.google.com/file/d/19QP-L1d3P_AJ0RVjJHLDo9Yet7WLH0A2/view?usp=sharing

These files have been tested and used to generate a live ELK deployment on Azure. They can be used to either recreate the entire deployment pictured above. Alternatively, select portions of the _____ file may be used to install only certain pieces of it, such as Filebeat.

 /etc/ansible/filebeat-playbook.yml
/etc/ansible/metricbeat-playbook.yml

This document contains the following details:
- Description of the Topologu
- Access Policies
- ELK Configuration
  - Beats in Use
  - Machines Being Monitored
- How to Use the Ansible Build


### Description of the Topology

The main purpose of this network is to expose a load-balanced and monitored instance of DVWA, the D*mn Vulnerable Web Application.

Load balancing ensures that the application will be highly Functional, in addition to restricting _____ to the network
 What aspect of security do load balancers protect? What is the advantage of a jump box?_
- Load balancers reroute live traffic incase of DDos attack or other availability issues. Jump box allows us to protect our azure VMs and also allows us to monitor and log in our boxes.
Integrating an ELK server allows users to easily monitor the vulnerable VMs for changes to the Network and system logs
What does Filebeat watch for?
Monitoring log files and locations and fowards to elasticsearch.
What does Metricbeat record?
Fowards metrics and statistics to elasticsearch

The configuration details of each machine may be found below.
_Note: Use the [Markdown Table Generator](http://www.tablesgenerator.com/markdown_tables) to add/remove values from the table_.

| Name     | Function | IP Address | Operating System |
|----------|----------|------------|------------------|
| Jump Box | Gateway  | 10.0.0.5   | Linux            |
| Web-1    |Ubuntuserver | 10.0.0.9 | Linux                       
| Web-2    |Ubuntuserver | 10.0.0.7 | Linux             
| ELKstack |Ubuntuserver | 10.1.0.4 | Linux                               

### Access Policies

The machines on the internal network are not exposed to the public Internet. 

Only the JumpBoxProvisioner machine can accept connections from the Internet. Access to this machine is only allowed from the following IP addresses:
- _TODO: 40.122.65.233: 5601 

Machines within the network can only be accessed by my personal workstation and SSH through jump-box.
- _TODO: Which machine did you allow to access your ELK VM? What was its IP address?_Jump-box-provisioner IP: 10.1.0.4 SSH port 22

A summary of the access policies in place can be found in the table below.

| Name     | Publicly Accessible | Allowed IP Addresses |
|----------|---------------------|----------------------|
| Jump Box | Yes                 | 71.201.222.245   
| Web-1    |  No                 |  10.1.0.4 ssh 22     |
| Web-2    |  No                 | 10.1.0.4 ssh 22      |
| ELKserver| No                  |  My public IP using TCP 5601
### Elk Configuration

Ansible was used to automate configuration of the ELK machine. No configuration was performed manually, which is advantageous because...
- _TODO: What is the main advantage of automating configuration with Ansible?_
- Ansible helps get your systems in a functioning state. Also YAML playbooks are very conveinient for quick and easy automation.
The playbook implements the following tasks:
- _TODO: In 3-5 bullets, explain the steps of the ELK installation play. E.g., install Docker; download image; etc._
- Configuring ELK:  
 - name: Config elk VM with Docker
    hosts: elk
    become: true
    tasks:
- Install Docker.io
  - name: Install docker.io
    apt:
      update_cache: yes
      force_apt_get: yes
      name: docker.io
      state: present
-Install Python-pip
  - name: Install python3-pip
    apt:
      force_apt_get: yes
      name: python3-pip
      state: present

    # Use pip module (It will default to pip3)
  - name: Install Docker module
    pip:
      name: docker
      state: present
      `docker`, which is the Docker Python pip module.

-Increase Virtual Memory
 - name: Use more memory
   sysctl:
     name: vm.max_map_count
     value: '262144'
     state: present
     reload: yes
-Download and Launch ELK Docker Container (image sebp/elk)
 - name: Download and launch a docker elk container
   docker_container:
     name: elk
     image: sebp/elk:761
     state: started
     restart_policy: always
-Published ports 5044, 5601 and 9200 were made available
     published_ports:
       -  5601:5601
       -  9200:9200
       -  5044:5044   

The following screenshot displays the result of running `docker ps` after successfully configuring the ELK instance.

(Images/docker_ps_output.png)

### Target Machines & Beats
This ELK server is configured to monitor the following machines:
List the IP addresses of the machines you are monitoring_
-Web-1: 10.0.0.9 and Web-2:10.0.0.7
We have installed the following Beats on these machines:
Specify which Beats you successfully installed_
-https://www.icloud.com/iclouddrive/0a0seTAtIrWYntT6Ii0hqojjA#Screen_Shot_2022-02-24_at_8.05.17_PM
-https://www.icloud.com/iclouddrive/0c1S4GH_wbonhMjZHJN3N-q7A#Screen_Shot_2022-02-24_at_9.58.16_PM
These Beats allow us to collect the following information from each machine:
 In 1-2 sentences, explain what kind of data each beat collects, and provide 1 example of what you expect to see. E.g., `Winlogbeat` collects Windows logs, which we use to track user logon events, etc._
- Filebeat is used to collect log files from specific systems or files like Microsoft azure, webservers, MySQL databases, and Apache.
-Metricbeat is used to monitor stats such as CPU, memory stats, network stats, and VM stats.
### Using the Playbook
In order to use the playbook, you will need to have an Ansible control node already configured. Assuming you have such a control node provisioned: 

SSH into the control node and follow the steps below:
- Copy the YML file to ansible.
- Update the config file to include hosts and ports
- Run the playbook, and navigate to Kibana to check that the installation worked as expected.

 Answer the following questions to fill in the blanks:_
- _Which file is the playbook? Where do you copy it? For ansible: install-elk.yml 
For filebeat: filebeat-playbook.yml
for Metricbeat: metricbeat-playbook.yml
- You copy them to /etc/ansible
- _Which file do you update to make Ansible run the playbook on a specific machine? How do I specify which machine to install the ELK server on versus which to install Filebeat on?
/etc/ansible/hosts
You have two seperate groups Webservers for the Vms I created, and ELK for my ELKserver.
- _Which URL do you navigate to in order to check that the ELK server is running?
137.117.19.231:5601/app/kibana
_As a **Bonus**, provide the specific commands the user will need to run to download the playbook, update the files, etc._
