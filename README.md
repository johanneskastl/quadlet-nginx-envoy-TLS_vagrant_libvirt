# Vagrant setup for running nginx behind envoy via quadlet

This setup prepares on CentOS9 Stream VM and then runs
an Ansible playbook, inspired by ygalblum's
[quadlet-demo](https://github.com/ygalblum/quadlet-demo).

This will install Podman on the VM and then configure
things using Quadlet, so in the end there is a Nginx
container running behind an envoy container, with the latter
listening on port 8080 (HTTP) and 8443 (HTTPS).

To reach the container from the machine running vagrant
you need to find out the Vagrant VM's IP address, e.g.
by using the vagrant address plugin or roaming through
the Ansible inventory file in
`.vagrant/provisioners/ansible/inventory/vagrant_ansible_inventory`.

Then run a simple `curl IP:8080` and you get the
default NGINX page. Run `curl -k https://IP:8443` and you get the same page via HTTPS (with a self-signed certificate, hence the `-k`).
Hooray!

## Vagrant

1. You need vagrant obviously. And Ansible.
2. Fetch the box, per default this is `generic/centos9s`, using `vagrant box add generic/centos9s`.
3. Make sure the git submodules are fully working by issuing `git submodule init && git submodule update`
4. Run `vagrant up`
5. Run a curl command against the VM's IP address on port 8080 (HTTP) or 8443 (HTTPS).
6. Party!

## Disabling the Ansible provisioning

In case you do not want Ansible to setup Podman (because you want to install it yourself), just comment out the following lines in the `Vagrantfile`:
```
    node.vm.provision "ansible" do |ansible|
      ansible.compatibility_mode = "2.0"
      ansible.playbook = "ansible/playbook-vagrant.yml"
    end # node.vm.provision
```
