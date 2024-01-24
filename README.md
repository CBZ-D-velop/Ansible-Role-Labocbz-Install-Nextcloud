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
![Tag: Nextcloud](https://img.shields.io/badge/Tech-Nextcloud-orange)

An Ansible role to install and configure a Nextcloud server based on Docker on your hosts.

This Ansible role simplifies the process of installing Nextcloud, an online storage service, at a specified location. It supports configuration to be served either by a web server handling PHP or by PHP FPM. The installation is performed with a specified version of Nextcloud using official server binaries.

The role generates the config.php configuration file for the initial launch. During installation via the web interface, Nextcloud performs database migration, adding cryptographic information such as salt and secret.

After installation, this role allows for rerunning the process to update parameters, for instance, adding a Redis server. Additionally, you can define, after installation by importing values from the config.php edited during web installation, the generated secret, salt, and instanceid. This prevents Nextcloud from requiring a reinstallation of the platform.

Database management is integrated, taking into account SSL and mTLS options, allowing for customization based on specific needs. Similarly, Redis management is included, though SSL and mTLS options are not supported.

It's important to note that this role does not handle the creation of users or databases for MySQL or any other database management system, and it does not manage the installation of a web server.

Utilize this Ansible role to efficiently automate the installation of Nextcloud, ensuring a customizable configuration to meet your specific needs, including secure management of the database and Redis.

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
install_nextcloud_install_path: "/var/www/html/my-nextcloud-server.domain.tld"
install_nextcloud_install_data_path: "/var/lib/nextcloud"
install_nextcloud_apache_system_user: "www-data"
install_nextcloud_apache_system_group: "www-data"
install_nextcloud_version: "27.1.5"

install_nextcloud_default_locale: "en_US"
install_nextcloud_default_phone_region: "GB"
install_nextcloud_force_locale: "{{ install_nextcloud_default_locale }}"
install_nextcloud_default_timezone: "Europe/Berlin"
install_nextcloud_loglevel: "0"
install_nextcloud_debug: "true"

install_nextcloud_admin_user: "nextcloud"
install_nextcloud_admin_password: "nextcloud"

install_nextcloud_mysql_database: "nextcloud"
install_nextcloud_mysql_user: "nextcloud"
install_nextcloud_mysql_password: "password"
install_nextcloud_mysql_host: "127.0.0.1"
install_nextcloud_mysql_port: "3306"

install_nextcloud_mysql_attr_ssl_key: "/var/www/html/my-nextcloud-server.domain.tld/ssl/my-nextcloud-server.domain.tld/my-nextcloud-server.domain.tld.pem.key"
install_nextcloud_mysql_attr_ssl_crt: "/var/www/html/my-nextcloud-server.domain.tld/ssl/my-nextcloud-server.domain.tld/my-nextcloud-server.domain.tld.pem.crt"
install_nextcloud_mysql_attr_ssl_ca: "/var/www/html/my-nextcloud-server.domain.tld/ssl/my-nextcloud-server.domain.tld/ca-chain.pem.crt"
install_nextcloud_mysql_attr_ssl_verify_server_cert: "true"

install_nextcloud_redis_host: "{{ inventory_hostname }}"
install_nextcloud_redis_port: "6379"
install_nextcloud_redis_password: "mySecret"

install_nextcloud_clean_cron_file: "ansible_nextcloud_cron_job"

# AFTER INSTALL IF MODIFICATIONS ARE NEEDED
#install_nextcloud_instanceid: "ocomj487bj5b"
#install_nextcloud_passwordsalt: "PswA+Tk6fEPn6ae0wXtxEhapv2f8ti"
#install_nextcloud_secret: "KqazwWa7/nkjXMAkgMlPQv1T+tp0oDLvfytrLC6VWtQYn+Um"

install_nextcloud_mail_from_address: "my.nextcloud" #@my.domain.tld
install_nextcloud_mail_smtpmode: "smtp"
install_nextcloud_mail_sendmailmode: "smtp"
install_nextcloud_mail_domain: "my.domain.tld"
install_nextcloud_mail_smtphost: "127.0.0.1"
install_nextcloud_mail_smtpport: 25

install_nextcloud_trusted_proxies:
  - "127.0.0.1"
  - "localhost"
install_nextcloud_trusted_domains:
  - "127.0.0.1"
  - "localhost"

install_nextcloud_overwritehost: "localhost"
install_nextcloud_owerwriteprotocol: "https"
install_nextcloud_owerwritecliurl: "{{ install_nextcloud_owerwriteprotocol }}://{{ install_nextcloud_overwritehost }}"

```

The best way is to modify these vars by copy the ./default/main.yml file into the ./vars and edit with your personnals requirements.

You can set vars in the template model in Ansible AWX / Tower or just surchage them during the playbook call.

In order to surchage vars, you have multiples possibilities but for mains cases you have to put vars in your inventory and/or on your AWX / Tower interface.

```YAML
# From inventory
---
inv_install_nextcloud_install_path: "/var/www/html/my-nextcloud-server.domain.tld"
inv_install_nextcloud_install_data_path: "/var/lib/nextcloud"
inv_install_nextcloud_apache_system_user: "www-data"
inv_install_nextcloud_apache_system_group: "www-data"
inv_install_nextcloud_version: "27.1.5"

inv_install_nextcloud_default_locale: "en_US"
inv_install_nextcloud_default_phone_region: "GB"
inv_install_nextcloud_force_locale: "{{ inv_install_nextcloud_default_locale }}"
inv_install_nextcloud_default_timezone: "Europe/Berlin"
inv_install_nextcloud_loglevel: "0"
inv_install_nextcloud_debug: "true"

inv_install_nextcloud_admin_user: "nextcloud"
inv_install_nextcloud_admin_password: "nextcloud"

inv_install_nextcloud_mysql_database: "nextcloud"
inv_install_nextcloud_mysql_user: "nextcloud"
inv_install_nextcloud_mysql_password: "password"
inv_install_nextcloud_mysql_host: "127.0.0.1"
inv_install_nextcloud_mysql_port: "3306"

inv_install_nextcloud_mysql_attr_ssl_key: "/var/www/html/my-nextcloud-server.domain.tld/ssl/my-nextcloud-server.domain.tld/my-nextcloud-server.domain.tld.pem.key"
inv_install_nextcloud_mysql_attr_ssl_crt: "/var/www/html/my-nextcloud-server.domain.tld/ssl/my-nextcloud-server.domain.tld/my-nextcloud-server.domain.tld.pem.crt"
inv_install_nextcloud_mysql_attr_ssl_ca: "/var/www/html/my-nextcloud-server.domain.tld/ssl/my-nextcloud-server.domain.tld/ca-chain.pem.crt"
inv_install_nextcloud_mysql_attr_ssl_verify_server_cert: "true"

inv_install_nextcloud_redis_host: "{{ inventory_hostname }}"
inv_install_nextcloud_redis_port: "6379"
inv_install_nextcloud_redis_password: "mySecret"

# AFTER INSTALL IF MODIFICATIONS ARE NEEDED
inv_install_nextcloud_instanceid: "ocrxwfyhv7ti"
inv_install_nextcloud_passwordsalt: "6iQBL9eRly9Mf+dhK1WUT80CVo+tJd"
inv_install_nextcloud_secret: "y/8CCjRqJKLORuivu393EoFORvaVGNm1rY58VEYR6ZILc55I"

inv_install_nextcloud_clean_cron_file: "ansible_nextcloud_cron_job"

inv_install_nextcloud_mail_from_address: "my.nextcloud" #@my.domain.tld
inv_install_nextcloud_mail_smtpmode: "smtp"
inv_install_nextcloud_mail_sendmailmode: "smtp"
inv_install_nextcloud_mail_domain: "my.domain.tld"
inv_install_nextcloud_mail_smtphost: "127.0.0.1"
inv_install_nextcloud_mail_smtpport: 25

inv_install_nextcloud_trusted_proxies:
  - "127.0.0.1"
  - "localhost"
inv_install_nextcloud_trusted_domains:
  - "127.0.0.1"
  - "localhost"

inv_install_nextcloud_overwritehost: "localhost"
inv_install_nextcloud_owerwriteprotocol: "https"
inv_install_nextcloud_owerwritecliurl: "{{ inv_install_nextcloud_owerwriteprotocol }}://{{ inv_install_nextcloud_overwritehost }}"

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
    install_nextcloud_install_path: "{{ inv_install_nextcloud_install_path }}"
    install_nextcloud_install_data_path: "{{ inv_install_nextcloud_install_data_path }}"
    install_nextcloud_apache_system_user: "{{ inv_install_nextcloud_apache_system_user }}"
    install_nextcloud_apache_system_group: "{{ inv_install_nextcloud_apache_system_group }}"
    install_nextcloud_version: "{{ inv_install_nextcloud_version }}"
    install_nextcloud_default_locale: "{{ inv_install_nextcloud_default_locale }}"
    install_nextcloud_default_phone_region: "{{ inv_install_nextcloud_default_phone_region }}"
    install_nextcloud_default_timezone: "{{ inv_install_nextcloud_default_timezone }}"
    install_nextcloud_loglevel: "{{ inv_install_nextcloud_loglevel }}"
    install_nextcloud_debug: "{{ inv_install_nextcloud_debug }}"
    install_nextcloud_mysql_database: "{{ inv_install_nextcloud_mysql_database }}"
    install_nextcloud_mysql_user: "{{ inv_install_nextcloud_mysql_user }}"
    install_nextcloud_mysql_password: "{{ inv_install_nextcloud_mysql_password }}"
    install_nextcloud_mysql_host: "{{ inv_install_nextcloud_mysql_host }}"
    install_nextcloud_mysql_port: "{{ inv_install_nextcloud_mysql_port }}"
    install_nextcloud_mysql_attr_ssl_key: "{{ inv_install_nextcloud_mysql_attr_ssl_key }}"
    install_nextcloud_mysql_attr_ssl_crt: "{{ inv_install_nextcloud_mysql_attr_ssl_crt }}"
    install_nextcloud_mysql_attr_ssl_ca: "{{ inv_install_nextcloud_mysql_attr_ssl_ca }}"
    install_nextcloud_mysql_attr_ssl_verify_server_cert: "{{ inv_install_nextcloud_mysql_attr_ssl_verify_server_cert }}"
    install_nextcloud_redis_host: "{{ inv_install_nextcloud_redis_host }}"
    install_nextcloud_redis_port: "{{ inv_install_nextcloud_redis_port }}"
    install_nextcloud_redis_password: "{{ inv_install_nextcloud_redis_password }}"
    install_nextcloud_trusted_proxies: "{{ inv_install_nextcloud_trusted_proxies }}"
    install_nextcloud_trusted_domains: "{{ inv_install_nextcloud_trusted_domains }}"
    install_nextcloud_overwritehost: "{{ inv_install_nextcloud_overwritehost }}"
    install_nextcloud_owerwriteprotocol: "{{ inv_install_nextcloud_owerwriteprotocol }}"
    install_nextcloud_owerwritecliurl: "{{ inv_install_nextcloud_owerwritecliurl }}"
    install_nextcloud_admin_user: "{{ inv_install_nextcloud_admin_user }}"
    install_nextcloud_admin_password: "{{ inv_install_nextcloud_admin_password }}"
    install_nextcloud_clean_cron_file: "{{ inv_install_nextcloud_clean_cron_file }}"
    #
    install_nextcloud_mail_from_address: "{{ inv_install_nextcloud_mail_from_address }}"
    install_nextcloud_mail_smtpmode: "{{ inv_install_nextcloud_mail_smtpmode }}"
    install_nextcloud_mail_sendmailmode: "{{ inv_install_nextcloud_mail_sendmailmode }}"
    install_nextcloud_mail_domain: "{{ inv_install_nextcloud_mail_domain }}"
    install_nextcloud_mail_smtphost: "{{ inv_install_nextcloud_mail_smtphost }}"
    install_nextcloud_mail_smtpport: "{{ inv_install_nextcloud_mail_smtpport }}"
    #
    install_nextcloud_instanceid: "{{ inv_install_nextcloud_instanceid }}"
    install_nextcloud_passwordsalt: "{{ inv_install_nextcloud_passwordsalt }}"
    install_nextcloud_secret: "{{ inv_install_nextcloud_secret }}"
  ansible.builtin.include_role:
    name: "labocbz.install_nextcloud"
```

## Architectural Decisions Records

Here you can put your change to keep a trace of your work and decisions.

### 2024-01-14: First Init

* First init of this role with the bootstrap_role playbook by Lord Robin Crombez

### 2024-01-20: Nextcloud installed

* Role install and configure Nextcloud for the first lauch
* You can use your prefered version of NC with the version parametter
* You can run multiples times the role, by adding salt, secret and instanceid in role calling after the initial install phase
* You need to install a webserver
* You need to install the database server
* You need to install Redis, if you want

### 2024-01-23: Install with CLI

* Role install Nextcloud with CLI and command line
* Role add a cron task for cron.php

### 2024-01-24: Added Email support

* You can now add your emails informations
* Added lockfile for Redis

## Authors

* Lord Robin Crombez

## Sources

* [Ansible role documentation](https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_reuse_roles.html)
* [Ansible Molecule documentation](https://molecule.readthedocs.io/)
