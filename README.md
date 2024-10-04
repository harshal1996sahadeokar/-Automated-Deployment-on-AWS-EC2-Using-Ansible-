# -Automated-Deployment-on-AWS-EC2-Using-Ansible-

Automated Deployment on AWS EC2 Using Ansible
Automating deployment using Ansible on AWS EC2 instances is a powerful solution for simplifying configuration management and application deployment. Ansible, as an open-source automation tool, allows you to automate the provisioning, configuration, and deployment of applications on EC2 instances.

In this guide, we'll explain the setup in detail, including the prerequisites, Ansible configuration, and deployment process.

Step-by-Step Detailed Explanation:
1. Prerequisites:
Before diving into the automation steps, you need the following:

AWS Account: Ensure you have access to AWS, with the necessary permissions to create EC2 instances and manage SSH access.
EC2 Key Pair: You need a key pair for connecting to your EC2 instances.
Ansible Installed: Install Ansible on your local machine or a control server.
On Linux/MacOS, you can install Ansible via the command:
bash
Copy code
sudo apt update && sudo apt install ansible -y
Python Installed on EC2 Instances: Ansible requires Python to run its playbooks, so ensure it’s installed on your EC2 instances.
You can install it via:
bash
Copy code
sudo yum install python3 -y
2. Set Up Your AWS EC2 Instances
Launch EC2 Instances:
Use the AWS Management Console, AWS CLI, or Terraform to launch EC2 instances.
Ensure that SSH access is configured and security groups allow inbound SSH connections from your control machine.
3. Configure Ansible Inventory File
Ansible uses an inventory file to define which servers to manage. Typically, this is a simple text file that lists EC2 instance IP addresses or domain names.

Create an Inventory File:

For example, in /etc/ansible/hosts:
ini
Copy code
[web]
ec2-11-22-33-44.compute-1.amazonaws.com
ec2-55-66-77-88.compute-1.amazonaws.com
Replace the IP addresses with your actual EC2 instance addresses.
Group Your Servers: You can group EC2 instances based on their roles (e.g., web, database):

ini
Copy code
[web]
ec2-web-1.compute-1.amazonaws.com
ec2-web-2.compute-1.amazonaws.com

[db]
ec2-db-1.compute-1.amazonaws.com
4. Create Ansible Playbook for Deployment
An Ansible playbook defines the tasks you want to automate. Let’s assume you’re deploying an Nginx web server on your EC2 instances. Below is a sample playbook that installs and starts Nginx:

nginx.yml:
yaml
Copy code
---
- hosts: web
  become: true
  tasks:
    - name: Update apt cache and install nginx
      apt:
        name: nginx
        state: present
        update_cache: true
  
    - name: Start nginx service
      service:
        name: nginx
        state: started
        enabled: true
Explanation:
hosts: This is the group of servers from the inventory file where the tasks will be executed (in this case, the "web" group).
become: true: This escalates privileges to root (sudo) on the EC2 instances.
tasks: These are the actions to be performed, such as updating the package manager and installing Nginx.
