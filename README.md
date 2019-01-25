<h1 align="center">Ansible examples</h1>

[![](https://cdn.iconscout.com/icon/free/png-256/ansible-282283.png)](https://www.ansible.com/)

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
## Modules

Ansible ships with a number of modules (called the ‘module library’) that can be executed directly on remote hosts or through Playbooks.

Users can also write their own modules. These modules can control system resources, like services, packages, or files (anything really), or handle executing system commands.

Modules (also referred to as “task plugins” or “library plugins”) are discrete units of code that can be used from the command line or in a playbook task.

Let’s review how we execute three different modules from the command line:

```zh
ansible webservers -m service -a "name=httpd state=started"
ansible webservers -m ping
ansible webservers -m command -a "/sbin/reboot -t now"
```

Each module supports taking arguments. Nearly all modules take key=value arguments, space delimited. Some modules take no arguments, and the command/shell modules simply take the string of the command you want to run.

From playbooks, Ansible modules are executed in a very similar way:
```yml
- name: reboot the servers
  action: command /sbin/reboot -t now
```
All modules technically return JSON format data, though if you are using the command line or playbooks, you don’t really need to know much about that. If you’re writing your own module, you care, and this means you do not have to write modules in any particular language – you get to choose.

[Documentation](https://docs.ansible.com/ansible/latest/user_guide/modules_intro.html)

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

## Ansible Tower
**Ansible Tower** helps you scale IT automation, manage complex deployments and speed productivity. Centralize and control your IT infrastructure with a visual dashboard, role-based access control, job scheduling, integrated notifications and graphical inventory management. And Ansible Tower's REST API and CLI make it easy to embed Ansible Tower into existing tools and processes.

The Ansible Tower dashboard provides a heads-up NOC-style display for everything going on in your Ansible environment.

As soon as you log in, you'll see your host and inventory status, all the recent job activity and a snapshot of recent job runs. Adjust your job status settings to graph data from specific job and time ranges.
![](https://www.ansible.com/hs-fs/hubfs/2018_Images/Tower-Product-Screens/Tower-3-3-Dashboard-2x.png?width=598&height=422&name=Tower-3-3-Dashboard-2x.png)

[Documentation](https://www.ansible.com/products/tower)
## License
Copyright © 2015-2018 Codica. It is released under the [MIT License](https://opensource.org/licenses/MIT).

## About Codica

[![Codica logo](https://www.codica.com/assets/images/logo/logo.svg)](https://www.codica.com)

The names and logos for Codica are trademarks of Codica.

We love open source software! See [our other projects](https://github.com/codica2) or [hire us](https://www.codica.com/) to design, develop, and grow your product.