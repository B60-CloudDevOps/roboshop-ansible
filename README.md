### Executive Summary — roboshop-ansible

The `roboshop-ansible` repository is intended to serve as a centralized platform for learning Ansible basics and implementing Configuration Management for all Roboshop components within the `roboshop/` directory.

1. **Modern Configuration Management with Ansible**
   Ansible enables infrastructure and application automation in a scalable, declarative, and platform-independent manner, reducing the operational complexity associated with traditional Bash scripting.

2. **Supports Both Push and Pull Automation Models**
   Ansible is one of the few Configuration Management tools that supports both:

   * **Push Mechanism** — where a centralized Ansible control node connects to target servers over SSH (port 22) and pushes configurations remotely.
   * **Pull Mechanism** — where managed nodes independently pull configurations from a Git repository and apply them locally.

3. **Push Mechanism Characteristics**
   Push-based automation requires:

   * Ansible installed on the central control node.
   * Network connectivity between the control node and managed servers.
   * Proper SSH authentication and credentials management, commonly through a dedicated `ansible-user`.
   * Stable or known server IPs and network accessibility.

4. **Pull Mechanism Characteristics**
   Pull-based automation requires:

   * Ansible installed directly on the managed node.
   * Connectivity from the node to the Git repository containing automation code.
   * Distributed execution capability without depending on centralized inbound connectivity.

5. **Declarative Automation Using YAML Playbooks**
   Unlike Bash, where automation logic is written as imperative scripts, Ansible uses **Playbooks** written in YAML (Yet Another Markup Language). YAML-based automation is declarative, human-readable, easier to maintain, and better suited for large-scale infrastructure management.


How to install ANSIBLE ?
   ANSIBLE is a python based package. All the python based packages are installed using pip3.x

   That means, we need to install pip and then using pip we will install ansible.

   With "# dnf install ansible -y" you get ansible-core which is ansible-core 2.14 ( redhast version 7). If you want a latest version of redhat ansible, install it using "pypi"

   PyPI (Python Package Index) is the official online repository and central marketplace for third-party software packages written in the Python programming language. It allows developers to easily find, share, and install pre-built code so they don't have to reinvent the wheel

   From this reference: https://pypi.org/project/ansible/

   Latest version of ansible needs minimum of python3.12,

   Ensure you do a "dnf remove python -y" and install python3.12
   # dnf install python3.12 -y
   # pip3.11 install ansible -y 
   ansible [core 2.15.13] = Redhat Ansible Verison 8

How ansible knows the information of the servers you're dealing with ?
   We need to source the IP or domain name of those servers in a file and that file is called as "Inventory"

Ansible is all about modules and they are readily available and can be referred from the documentation.


Ansible commands can be executed in 2 ways :
   1) Using Ad-hoc based commands and with adhoc based commands you cannot run more than one instruction at a time.
      "ansible -i 172.31.22.44,172.31.22.26, all -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ansible.builtin.ping"
         ansible_user, ansible_password are the pre-defined commands.
      "ansible -i inventory all -e ansible_user=ec2-user -e ansible_password=DevOps321 -m ansible.builtin.ping"
      what is all here ? all is the group that includes every entry in the inventory file