# halcyon-vagrant-kubernetes
Please see [release notes](https://github.com/att-comdev/halcyon-vagrant-kubernetes/releases) for current and planned features.

A Vagrant deployment of [halcyon-kubernetes](https://github.com/att-comdev/halcyon-kubernetes) -- Ansible playbooks for a kubadm-based kubernetes deployment, supporting any cloud and any kubeadm-enabled OS.


## Requirements

  * Virtualbox 5.0 (5.2 will not work with Vagrant)
  * Virtualbox Extensions for 5.0
  * Ansible (version 2.1.1 and up tested)
  * Vagrant (1.8.4 or higher, but **not 1.9 as there are known issues**)
    - Following Vagrant Plugins (installed for you during first deploy):
      * vagrant-env
      * vagrant-git
      * vagrant-openstack-provider

Please see /docs/README.md for more information about SDN providers, plugins, and other useful information. Pull requests are welcome!


## Instructions
To use this project, simply use vagrant to bring up your environment:

```
v1k0d3n@machine:~ $ git clone https://github.com/att-comdev/halcyon-vagrant-kubernetes.git
v1k0d3n@machine:~ $ cd halcyon-vagrant-kubernetes
v1k0d3n@machine:~ $ git submodule init
v1k0d3n@machine:~ $ git submodule update
v1k0d3n@machine:~ $ vagrant up
```

### Configuration Helper
A helper script is provided to set up basic common configuration options, it can
be used to change the guest OS and Kubernetes version. It also supports changing
between the default halcyon-kubernetes config and one optimized for OpenStack
Kolla-Kubernetes development. For example, to setup a Kolla development environment
running CentOS and Kubernetes v1.4.6 can simply be achieved by running:

```
v1k0d3n@machine:~ $ ./setup-halcyon.sh --guest-os centos --k8s-config kolla --k8s-version v1.4.6
```


### Deploy directly to Openstack:
When you want to use Openstack, edit the options in `./config.rb` to match your Openstack project, and deploy with the `--provider=openstack` flag:

```
v1k0d3n@machine:~ $ vagrant up --provider=openstack
```

### Deploy using Libvirt:
If you would like to use libvirt rather than virtualbox, install the follow vagrant plugin and deploy with the `--provider=libvirt` flag:

```
v1k0d3n@machine:~ $ vagrant plugin install vagrant-libvirt
v1k0d3n@machine:~ $ vagrant up --provider=libvirt
```

NOTE: Please look over the options in `config.rb` for modifying number of nodes, subnet, and other information and if you want to make any modifications to the Ansible deployment, make changes to the `./kube-deploy/group_vars/all.yml` file.


### Accessing the cluster:
To access the deployed cluster either log in to the node `kube1`:

```
v1k0d3n@machine:~ $ vagrant ssh kube1
```
or if kubectl is installed locally on the development host, you can alternatively use the provided helper-script to access the cluster:
```
v1k0d3n@machine:~ $ ./get-k8s-creds.sh
```

### Ubuntu Deployment Issues:

If you are deploying project on a Ubuntu 16.04+ host, you may need to install the following dependencies to ensure that the Vagrant plugins get installed properly:

`sudo apt-get install ruby-dev zlib1g-dev libgmp-dev libxml2-dev libssl-dev openssl libffi-dev`


# TODO

* Add conditionals for various deployments (using vagrant --provider flags; such as AWS provider).
