# Ansible role: labocbz.add_php_fpm_confs

![Licence Status](https://img.shields.io/badge/licence-MIT-brightgreen)
![CI Status](https://img.shields.io/badge/CI-success-brightgreen)
![Testing Method](https://img.shields.io/badge/Testing%20Method-Ansible%20Molecule-blueviolet)
![Testing Driver](https://img.shields.io/badge/Testing%20Driver-docker-blueviolet)
![Language Status](https://img.shields.io/badge/language-Ansible-red)
![Compagny](https://img.shields.io/badge/Compagny-Labo--CBZ-blue)
![Author](https://img.shields.io/badge/Author-Lord%20Robin%20Crombez-blue)

## Description

![Tag: Ansible](https://img.shields.io/badge/Tech-Ansible-orange)
![Tag: Debian](https://img.shields.io/badge/Tech-Debian-orange)
![Tag: PHP](https://img.shields.io/badge/Tech-PHP-orange)
![Tag: PHP-FPM](https://img.shields.io/badge/Tech-PHP--FPM-orange)

An Ansible role to import and add PHP FPM configurations in an existing PHP FPM server.

Simplify PHP-FPM management with this Ansible Role. This role is designed to efficiently import and configure PHP-FPM pools, allowing you to tailor your PHP-FPM environment according to your specific needs.

With this role, you can easily define the PHP version and create custom pools without hassle. Fine-tune resource allocation, server thresholds, and other parameters to optimize performance. Additionally, user and group ownership can be configured effortlessly, enhancing security and ensuring controlled access to the pools.

One of the key features of this role is the seamless setup of sockets for PHP-FPM communication. The role automates the creation of the required sockets directory, making the process smooth and hassle-free.

Streamline your PHP-FPM configuration and optimize resource utilization using our Ansible Role. Enjoy a simplified and efficient PHP-FPM management process, allowing your web applications to run smoothly and securely.

## Folder structure

By default Ansible will look in each directory within a role for a main.yml file for relevant content (also man.yml and main):

```PYTHON
.
├── README.md  # Contains an overview of the role and its purpose.
├── defaults
│   ├── main.yml  # Contains default variables for the role that can be overridden by users.
│   └── README.md  # Contains documentation for the default variables.
├── files
│   └── README.md  # Contains documentation for the files in the directory.
├── handlers
│   ├── main.yml  # Contains handlers that can be called by tasks within the role.
│   └── README.md  # Contains documentation for the handlers.
├── meta
│   ├── main.yml  # Contains metadata about the role, including dependencies and supported platforms.
│   └── README.md  # Contains documentation for the role metadata.
├── tasks
│   ├── main.yml  # Contains tasks to be executed by the role on the managed nodes.
│   └── README.md  # Contains documentation for the tasks.
├── templates
│   └── README.md  # Contains documentation for the templates.
└── vars
    ├── main.yml  # Contains variables that are specific to the role and are not meant to be overridden.
    └── README.md  # Contains documentation for the role variables.
```

## Tests and simulations

### Basics

You have to run multiples tests. *tests with an # are mandatory*

```MARKDOWN
# lint
# syntax
# converge
# idempotence
# verify
side_effect
```

Executing theses test in this order is called a "scenario" and Molecule can handle them.

Molecule use Ansible and pre configured playbook to create containers, prepare them, converge (run the role) and verify its execution.
You can manage multiples scenario with multiples tests in order to get a 100% code coverage.

This role contains a ./tests folder. In this folder you can use the inventory or the tower folder to create a simualtion of a real inventory and a real AWX / Tower job execution.

### Command reminder

```SHELL
# Check your YAML syntax
yamllint -c ./.yamllint .

# Check your Ansible syntax and code security
ansible-lint --config=./.ansible-lint .

# Execute and test your role
molecule lint
molecule create
molecule list
molecule converge
molecule verify
molecule destroy

# Execute all previous task in one single command
molecule test
```

## Installation

To install this role, just copy/import this role or raw file into your fresh playbook repository or call it with the "include_role/import_role" module.

## Usage

### Vars

Some vars a required to run this role:

```YAML
---
add_php_fpm_confs_php_version: "8.2"
add_php_fpm_confs_php_pools_path: "/etc/php/{{ add_php_fpm_confs_php_version }}/fpm/pool.d"

add_php_fpm_confs_fpm_pools:
  - name: "www"
    user: "www-data"
    group: "www-data"
    mode: "0600"
    max_children: 40
    start_servers: 8
    min_spare_servers: 4
    max_spare_servers: 4
    process_idle_timeout: "10s"
    max_requests: 500

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_add_php_fpm_confs_php_version: "8.2"
inv_add_php_fpm_confs_php_pools_path: "/etc/php/{{ inv_add_php_fpm_confs_php_version }}/fpm/pool.d"

inv_add_php_fpm_confs_fpm_pools:
  - name: "www"
    user: "www-data"
    group: "www-data"
    mode: "0600"
    max_children: 40
    start_servers: 8
    min_spare_servers: 4
    max_spare_servers: 4
    process_idle_timeout: "10s"
    max_requests: 500

  - name: "PhpMyAdmin"
    user: "www-data"
    group: "www-data"
    mode: "0600"
    max_children: 40
    start_servers: 8
    min_spare_servers: 4
    max_spare_servers: 4
    process_idle_timeout: "10s"
    max_requests: 500

  - name: "Symfony-1"
    user: "www-data"
    group: "www-data"
    mode: "0600"
    max_children: 40
    start_servers: 8
    min_spare_servers: 4
    max_spare_servers: 4
    process_idle_timeout: "10s"
    max_requests: 500

  - name: "Symfony-2"
    user: "www-data"
    group: "www-data"
    mode: "0600"
    max_children: 40
    start_servers: 8
    min_spare_servers: 4
    max_spare_servers: 4
    process_idle_timeout: "10s"
    max_requests: 500
```

```YAML
# From AWX / Tower

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.add_php_fpm_confs"
    tags:
    - "labocbz.add_php_fpm_confs"
    vars:
    add_php_fpm_confs_php_version: "{{ inv_add_php_fpm_confs_php_version }}"
    add_php_fpm_confs_php_pools_path: "{{ inv_add_php_fpm_confs_php_pools_path }}"
    add_php_fpm_confs_fpm_pools: "{{ inv_add_php_fpm_confs_fpm_pools }}"
    ansible.builtin.include_role:
    name: "labocbz.add_php_fpm_confs"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2023-08-12: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez
* Role import configuration of php fpm pool
* Role not install PHP
* Role create socket diretory

### 2023-10-06: New CICD, new Images

* New CI/CD scenario name
* Molecule now use remote Docker image by Lord Robin Crombez
* Molecule now use custom Docker image in CI/CD by env vars
* New CICD with needs and optimization

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
* [Best PHP-FPM Configuration – Easy and Simple Calculation](https://www.cloudbooklet.com/best-php-fpm-configuration-easy-and-simple-calculation/)