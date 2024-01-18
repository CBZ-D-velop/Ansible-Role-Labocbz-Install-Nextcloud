# Ansible role: labocbz.install_nextcloud

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
![Tag: Docker](https://img.shields.io/badge/Tech-Docker-orange)
![Tag: Nextcloud](https://img.shields.io/badge/Tech-Nextcloud-orange)

An Ansible role to install and configure a Nextcloud server based on Docker on your hosts.

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

install_nextcloud_container_name: "nextcloud"
install_nextcloud_data_path: "/var/lib/nextcloud"
install_nextcloud_web_address: "0.0.0.0"
install_nextcloud_web_port: 8080

install_nextcloud_mysql_database: "nextcloud"
install_nextcloud_mysql_user: "nextcloud"
install_nextcloud_mysql_password: "password"
install_nextcloud_mysql_host: "127.0.0.1"

#install_nextcloud_mysql_attr_ssl_key: ""
#install_nextcloud_mysql_attr_ssl_cert: ""
#install_nextcloud_mysql_attr_ssl_ca: ""
#install_nextcloud_mysql_attr_ssl_verify_server_cert: "true"

install_nextcloud_admin_user: "nextcloud"
install_nextcloud_admin_password: "password"

#install_nextcloud_redis_host: "127.0.0.1"
#install_nextcloud_redis_host_port: "6379"
#install_nextcloud_redis_host_password: "password"

install_nextcloud_php_memory_limit: "512m"
install_nextcloud_php_upload_limit: "512m"

install_nextcloud_apache_disable_rewrite_ip: 0
install_nextcloud_trusted_proxies: "127.0.0.1 localhost"
install_nextcloud_trusted_domains: "http://my.nextcloud.domain.tld 127.0.0.1 localhost"
install_nextcloud_overwritehost: "http://my.nextcloud.domain.tld"
install_nextcloud_owerwriteprotocol: "http"
install_nextcloud_owerwritecliurl: "{{ install_nextcloud_overwritehost }}"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---

inv_install_nextcloud_container_name: "nextcloud"
inv_install_nextcloud_data_path: "/var/lib/nextcloud"
inv_install_nextcloud_web_address: "0.0.0.0"
inv_install_nextcloud_web_port: 8080

inv_install_nextcloud_mysql_database: "nextcloud"
inv_install_nextcloud_mysql_user: "nextcloud"
inv_install_nextcloud_mysql_password: "password"
inv_install_nextcloud_mysql_host: "127.0.0.1"

#inv_install_nextcloud_mysql_attr_ssl_key: ""
#inv_install_nextcloud_mysql_attr_ssl_cert: ""
#inv_install_nextcloud_mysql_attr_ssl_ca: ""
#inv_install_nextcloud_mysql_attr_ssl_verify_server_cert: "true"

inv_install_nextcloud_admin_user: "nextcloud"
inv_install_nextcloud_admin_password: "password"

#inv_install_nextcloud_redis_host: "127.0.0.1"
#inv_install_nextcloud_redis_host_port: "6379"
#inv_install_nextcloud_redis_host_password: "password"

inv_install_nextcloud_php_memory_limit: "512m"
inv_install_nextcloud_php_upload_limit: "512m"

inv_install_nextcloud_apache_disable_rewrite_ip: 0
inv_install_nextcloud_trusted_proxies: "127.0.0.1 localhost"
inv_install_nextcloud_trusted_domains: "http://my.nextcloud.domain.tld 127.0.0.1 localhost"
inv_install_nextcloud_overwritehost: "http://my.nextcloud.domain.tld"
inv_install_nextcloud_owerwriteprotocol: "http"
inv_install_nextcloud_owerwritecliurl: "{{ inv_install_nextcloud_overwritehost }}"

```

```YAML
# From AWX / Tower
---

```

### Run

To run this role, you can copy the molecule/default/converge.yml playbook and add it into your playbook:

```YAML
- name: "Include labocbz.install_nextcloud"
  tags:
    - "labocbz.install_nextcloud"
  vars:
    install_nextcloud_container_name: "{{ inv_install_nextcloud_container_name }}"
    install_nextcloud_data_path: "{{ inv_install_nextcloud_data_path }}"
    install_nextcloud_web_address: "{{ inv_install_nextcloud_web_address }}"
    install_nextcloud_web_port: "{{ inv_install_nextcloud_web_port }}"
    install_nextcloud_mysql_database: "{{ inv_install_nextcloud_mysql_database }}"
    install_nextcloud_mysql_user: "{{ inv_install_nextcloud_mysql_user }}"
    install_nextcloud_mysql_password: "{{ inv_install_nextcloud_mysql_password }}"
    install_nextcloud_mysql_host: "{{ inv_install_nextcloud_mysql_host }}"
    install_nextcloud_mysql_attr_ssl_key: "{{ inv_install_nextcloud_mysql_attr_ssl_key }}"
    install_nextcloud_mysql_attr_ssl_cert: "{{ inv_install_nextcloud_mysql_attr_ssl_cert }}"
    install_nextcloud_mysql_attr_ssl_ca: "{{ inv_install_nextcloud_mysql_attr_ssl_ca }}"
    install_nextcloud_mysql_attr_ssl_verify_server_cert: "{{ inv_install_nextcloud_mysql_attr_ssl_verify_server_cert }}"
    install_nextcloud_admin_user: "{{ inv_install_nextcloud_admin_user }}"
    install_nextcloud_admin_password: "{{ inv_install_nextcloud_admin_password }}"
    install_nextcloud_redis_host: "{{ inv_install_nextcloud_redis_host }}"
    install_nextcloud_redis_host_port: "{{ inv_install_nextcloud_redis_host_port }}"
    install_nextcloud_redis_host_password: "{{ inv_install_nextcloud_redis_host_password }}"
    install_nextcloud_php_memory_limit: "{{ inv_install_nextcloud_php_memory_limit }}"
    install_nextcloud_php_upload_limit: "{{ inv_install_nextcloud_php_upload_limit }}"
    install_nextcloud_apache_disable_rewrite_ip: "{{ inv_install_nextcloud_apache_disable_rewrite_ip }}"
    install_nextcloud_trusted_proxies: "{{ inv_install_nextcloud_trusted_proxies }}"
    install_nextcloud_trusted_domains: "{{ inv_install_nextcloud_trusted_domains }}"
    install_nextcloud_overwritehost: "{{ inv_install_nextcloud_overwritehost }}"
    install_nextcloud_owerwriteprotocol: "{{ inv_install_nextcloud_owerwriteprotocol }}"
    install_nextcloud_owerwritecliurl: "{{ inv_install_nextcloud_owerwritecliurl }}"
  ansible.builtin.include_role:
    name: "labocbz.install_nextcloud"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2024-01-14: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
* [nextcloud](https://hub.docker.com/_/nextcloud)
* [IMPORTANT NOTE / nextcloud](https://github.com/docker-library/docs/blob/master/nextcloud/README.md)
