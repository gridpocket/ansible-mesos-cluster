# Deploy a Mesos / Marathon / Docker cluster with Ansible

![Mesos Architecture](https://assets.digitalocean.com/articles/mesosphere/mesos_architecture.png "Mesos Architecture")
(image from digitalocean)

## Current status

This recipe is not fully functional yet !!!  

When deploying a task from the marathon UI, the deployment hangs forever if mesos master is not the first one of the list (!). I'm currently digging into this bug.

## Quick start with Vagrant

Vagrantfile defines 6 VMs:  
- 3 mesos masters running mesos, marathon
- 3 mesos slaves running docker

1. vagrant up  
=> 6 VMs (with IP from 192.168.1.211 to 192.168.1.216) arre created  

2. ansible-playbook -i inventory/test.ini -k -u vagrant -s init.yml  
=> enter "vagrant" when password is required
=> "mesos" user (with authentication key and sudo rights) is created on each host  

3. ansible-playbook -i inventory/test.ini main.yml  
=> Installation and configuration of Zookeeper, Mesos masters, Mesos slaves (with Docker), Marathon

## Inventory

The host file defines the inventory/ENVIRONMENT.ini (mesos master and mesos slaves)

eg:  

    [zookeepers]
    192.168.1.211
    192.168.1.212
    192.168.1.213

    [mesos-masters]
    192.168.1.211
    192.168.1.212
    192.168.1.213

    [mesos-slaves]
    192.168.1.214
    192.168.1.215
    192.168.1.216

    [marathons]
    192.168.1.211
    192.168.1.212
    192.168.1.213

## Nodes initialisation

This first task initiate the server creating a user named mongors
- mesos user will be given sudo right with no password needed when running sudo commands
- current machine ssh key is copied over to the authorized_keys of the server that is beeing provisionned

Depending upon the node access, several bootstrap scenario can be used:

### - Usage of Vagrant VM

    ansible-playbook -i inventory/ENVIRONMENT.ini -k -u vagrant -s init.yml

note: vagrant ssh password will be requested (sudo password not requested as vagrant user is authorized to sudo without any password by default)

### - Usage of VPS with root access

    ansible-playbook -i inventory/ENVIRONMENT.ini -k -u root init.yml

note: root ssh password will be requested

### - Usage of a user with sudo rights

    ansible-playbook -i inventory/ENVIRONMENT.ini -k -K -u USER -s init.yml

note: both user's ssh password + sudo password will be requested

## Mesos cluster setup

Once the nodes are registered, the replica set can be created using the following command:

    ansible-playbook -i inventory/ENVIRONMENT.ini main.yml

## License

MIT License - Copyright © 2015 GridPocket

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the “Software”), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED “AS IS”, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
