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


# Executive Summary — Introduction to Ansible

## What is Ansible?

Ansible is an open-source automation and configuration management tool used for:

* Server provisioning
* Application deployment
* Configuration management
* Infrastructure automation

It is agentless, meaning no software needs to be installed on target servers. It primarily uses SSH for communication.

---

# Installing Ansible

## Core Concept

Ansible is a Python-based application and is commonly installed using Python package managers like `pip`.

### Default Installation via DNF

```bash
dnf install ansible -y
```

* This usually installs **ansible-core** from the OS repository.
* On RHEL/Rocky/AlmaLinux 9, this often provides:

  * `ansible-core 2.14`
  * Equivalent to Red Hat Ansible Automation Platform version 7-era tooling.

---

## Installing Latest Ansible via PyPI

[PyPI Ansible Package](https://pypi.org/project/ansible/?utm_source=chatgpt.com)

### Why PyPI?

PyPI provides the latest upstream Ansible versions faster than OS repositories.

### Python Requirement

Latest Ansible releases require newer Python versions (Python 3.12+ recommended).

### Suggested Installation Steps

```bash
dnf install python3.12 -y

pip3.12 install ansible
```

### Important Correction

The original note mentions:

```bash
dnf remove python -y
```

This is risky and not recommended because:

* System Python is required by the operating system.
* Removing it can break package management and core OS tools.

Instead:

* Install Python 3.12 alongside the existing version.
* Use `pip3.12`.

---

# Inventory in Ansible

Ansible manages servers using an **Inventory** file.

The inventory contains:

* IP addresses
* Hostnames
* Groups of servers

Example:

```ini
[dev]
172.31.22.44

[prod]
172.31.22.26
```

---

# How Ansible Works

Ansible operates mainly using:

1. **Ad-hoc Commands**
2. **Playbooks**

---

# 1) Ad-hoc Commands

Ad-hoc commands are:

* One-line commands
* Used for quick operations
* Best for testing or small tasks

## Example: Ping Servers

```bash
ansible -i inventory all \
-e ansible_user=ec2-user \
-e ansible_password=DevOps321 \
-m ansible.builtin.ping
```

### Key Components

| Component      | Meaning                |
| -------------- | ---------------------- |
| `-i inventory` | Inventory file         |
| `all`          | All hosts in inventory |
| `-e`           | Extra variables        |
| `-m`           | Module name            |

---

## Using Groups

```bash
ansible -i inventory prod \
-e ansible_user=ec2-user \
-e ansible_password=DevOps321 \
-m ansible.builtin.ping
```

Here:

* `prod` refers to a server group in inventory.

---

## Installing Packages via Ad-hoc Command

```bash
ansible -i inventory dev \
-e ansible_user=ec2-user \
-e ansible_password=DevOps321 \
-m ansible.builtin.package \
-a "name=nginx state=present" \
--become
```

### What this does

* Targets all servers in the `dev` group
* Installs `nginx`
* `--become` executes commands with elevated privileges (sudo/root)

---

# 2) Playbooks

Playbooks are YAML-based automation files that allow:

* Multiple tasks
* Sequential execution
* Reusability
* Infrastructure as Code (IaC)

Example capabilities:

* Install packages
* Configure services
* Deploy applications
* Restart services

---

# Key Takeaways

* Ansible is an agentless automation tool built on Python.
* Latest versions are best installed via PyPI using modern Python versions.
* Inventory files define managed servers and groups.
* Ad-hoc commands are suitable for quick operations.
* Playbooks provide scalable and reusable automation workflows.
* Modules are the building blocks of Ansible automation.


# To learn YAML ( Yet Another Markup Language ), we need need to know 4 important basics 
   1) Dictionary     ( a key with a single value is referred as dictionary )
      ```
         name: Martin D'vloper
         job: Developer
      ```
   2) List           ( a key with multiple values is referred as list )
      ```
         skills:
            - lisp
            - fortran
            - erlang
      ```
   3) Map            ( a key with multiple key value pairs is referred as Map )
   4) Each & every yaml file should end with .yml or .yaml
   5) Unline bash, YAML is indendation specific ( Either you use one or two spaces across the board )

What is a playbook ?
   Playbook is nothing but a list of plays.

What is a play ?
   A play is a list of tasks that has to be executed.

What is a task ?
   Task is something we want to execute.

How to run an ansible playbook ?
   "ansible-playbook -i inventory -e ansible_user=ec2-user -e ansible_password=DevOps321 01-playbook.yaml"

Variables Types:
   1) Play level variables
   2) Task level variable
   3) Command line variable 


   task variable > play level variable