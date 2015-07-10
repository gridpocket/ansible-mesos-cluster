# Deploy a Mesos / Marathon / Docker cluster with Ansible

![Mesos Architecture](https://assets.digitalocean.com/articles/mesosphere/mesos_architecture.png "Mesos Architecture")
(image from digitalocean)

## Current status

This recipe is a work in progress, a configuration bug needs to be fixed.  
Symptoms:  
  - Slaves not seen in the mesos UI

## Init Vagrant boxes and start VMs  

6 boxes declared in vagrant/Vagrantfile
- 3 mesos masters running mesos, marathon
- 3 mesos slaves running docker

## Typical usage with vagrant boxes

    1. Set ip in of the vagrant boxes in the inventory/ENVIRONMENT.ini  file

    2. Run: ansible-playbook -i inventory/ENVIRONMENT.ini -k -u vagrant -s init.yml
       Note: other options are detailled below to bootstrap this cluster on non vagrant boxes

    3. Run: ansible-playbook -i inventory/ENVIRONMENT.ini main.yml
       Note: mesos, marathon and docker tags are defined in the tasks.
             If only mesos (master, slave, zookeeper) needs to be ran, use the following command:  

    Run: ansible-playbook -i inventory/ENVIRONMENT.ini -t mesos main.yml

## Inventory

The host file defines the inventory/ENVIRONMENT.ini (mesos master and mesos slaves)

eg:  

    [mesos-masters]
    192.168.1.191
    192.168.1.192
    192.168.1.193

    [mesos-slaves]
    192.168.1.194
    192.168.1.195
    192.168.1.196

    [nodes:children]
    mesos-masters
    mesos-slaves

## Nodes initialisation

This first task initiate the server creating a user named mongors
- mongors user will be given sudo right with no password needed when running sudo commands
- current machine ssh key is copied over to the authorized_keys of the server that is beeing provisionned

Several possible cases

### Usage of Vagrant VM

    ansible-playbook -i inventory/ENVIRONMENT.ini -k -u vagrant -s init.yml

note: vagrant ssh password will be requested (sudo password not requested as vagrant user is authorized to sudo without any password by default)

### Usage of VPS with root access

    ansible-playbook -i inventory/ENVIRONMENT.ini -k -u root init.yml

note: root ssh password will be requested

### Usage of a user with sudo rights

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
