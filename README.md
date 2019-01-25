<h1 align="center">Ansible examples</h1>
![](https://upload.wikimedia.org/wikipedia/commons/thumb/2/24/Ansible_logo.svg/2000px-Ansible_logo.svg.png)

**Ansible** is a radically simple IT automation engine that automates cloud provisioning, configuration management, application deployment, intra-service orchestration, and many other IT needs.

## Instalation
### Latest Releases via Apt (Ubuntu)

```sh
$ sudo apt-get update
$ sudo apt-get install software-properties-common
$ sudo apt-add-repository --yes --update ppa:ansible/ansible
$ sudo apt-get install ansible
```

### Latest Releases via Apt (Debian)

Debian users may leverage the same source as the Ubuntu PPA.

Add the following line to /etc/apt/sources.list:

```sh
deb http://ppa.launchpad.net/ansible/ansible/ubuntu trusty main

```
Then run these commands:

```sh
$ sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 93C4A3FD7BB9C367
$ sudo apt-get update
$ sudo apt-get install ansible

```
[Github](https://github.com/ansible/ansible) and [Official Documentation](https://docs.ansible.com)

## Hosts

Ansible works against multiple systems in your infrastructure at the same time. It does this by selecting portions of systems listed in Ansible’s inventory, which defaults to being saved in the location ```/etc/ansible/hosts```. You can specify a different inventory file using the ```-i <path>``` option on the command line.

```
mail.example.com

[webservers]
foo.example.com
bar.example.com

[dbservers]
one.example.com
two.example.com
three.example.com
```
The headings in brackets are group names, which are used in classifying systems and deciding what systems you are controlling at what times and for what purpose.

A YAML version would look like:

```yaml
all:
  hosts:
    mail.example.com:
  children:
    webservers:
      hosts:
        foo.example.com:
        bar.example.com:
    dbservers:
      hosts:
        one.example.com:
        two.example.com:
        three.example.com:
```
[Documentation](https://docs.ansible.com/ansible/latest/user_guide/intro_inventory.html)
## Playbooks
**Playbooks** are Ansible’s configuration, deployment, and orchestration language. They can describe a policy you want your remote systems to enforce, or a set of steps in a general IT process.

If Ansible modules are the tools in your workshop, playbooks are your instruction manuals, and your inventory of hosts are your raw material.

Playbooks are expressed in YAML format (see YAML Syntax) and have a minimum of syntax, which intentionally tries to not be a programming language or script, but rather a model of a configuration or a process.

Each playbook is composed of one or more ‘plays’ in a list.

Here’s a playbook that contains just one play:

```yml
---
- hosts: webservers
  vars:
    http_port: 80
    max_clients: 200
  remote_user: root
  tasks:
  - name: ensure apache is at the latest version
    yum:
      name: httpd
      state: latest
  - name: write the apache config file
    template:
      src: /srv/httpd.j2
      dest: /etc/httpd.conf
    notify:
    - restart apache
  - name: ensure apache is running
    service:
      name: httpd
      state: started
  handlers:
    - name: restart apache
      service:
        name: httpd
        state: restarted
```
### Executing A Playbook

How do you run a playbook? It’s simple. Let’s run a playbook using a parallelism level of 10:

```zh
ansible-playbook playbook.yml -f 10
```
[Documentation](https://docs.ansible.com/ansible/latest/user_guide/playbooks.html)
## License
Copyright © 2015-2018 Codica. It is released under the [MIT License](https://opensource.org/licenses/MIT).

## About Codica

[![Codica logo](https://www.codica.com/assets/images/logo/logo.svg)](https://www.codica.com)

The names and logos for Codica are trademarks of Codica.

We love open source software! See [our other projects](https://github.com/codica2) or [hire us](https://www.codica.com/) to design, develop, and grow your product.