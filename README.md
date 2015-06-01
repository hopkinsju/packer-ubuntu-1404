# Packer Example - Ubuntu 14.04 minimal Vagrant Box

**Current Ubuntu Version Used**: 14.04.1

This example build configuration installs and configures Ubuntu 14.04 x86_64 minimal using Ansible, and then generates two Vagrant box files, for:

  - VirtualBox
  - VMware

The example can be modified to use more Ansible roles, plays, and included playbooks to fully configure (or partially) configure a box file suitable for deployment for development environments.

## Requirements

The following software must be installed/present on your local machine before you can use Packer to build the Vagrant box file:

  - [Packer](http://www.packer.io/)
  - [Vagrant](http://vagrantup.com/)
  - [VirtualBox](https://www.virtualbox.org/) (if you want to build the VirtualBox box)
  - [VMware Fusion](http://www.vmware.com/products/fusion/) (or Workstation - if you want to build the VMware box)
  - [Ansible](http://docs.ansible.com/intro_installation.html)

You will also need some Ansible roles installed so they can be used in the building of the VM. To install the roles:

  1. Run `$ ansible-galaxy install -r requirements.txt` in this directory.
  2. If your local Ansible roles path is not the default (`/etc/ansible/roles`), update the `role_paths` inside `ubuntu1404.json` to match your custom location.

If you don't have Ansible installed (perhaps you're using a Windows PC?), you can simply clone the required Ansible roles from GitHub directly (use [Ansible Galaxy](https://galaxy.ansible.com/) to get the GitHub repository URLs for each role listed in `requirements.txt`), and update the `role_paths` variable to match the location of the cloned role.

## Usage

### Packer Build

Make sure all the required software (listed above) is installed, then cd to the directory containing this README.md file, and run:

    $ packer build ubuntu1404.json

After a few minutes, Packer should tell you the box was generated successfully.

If you want to only build a box for one of the supported virtualization platforms (e.g. only build the VirtualBox box), add `--only=virtualbox-iso` to the `packer build` command:

    $ packer build --only=virtualbox-iso ubuntu1404.json

The ubuntu1404.json file specifies both a VMWare and VirtualBox build, and by default packer will build both boxes. Because some VMWare Vagrant providers use non-free plugins, from here on out we will only be working with VirtualBox.

#### Keeping an Eye on the Builds

The "Waiting for SSH to become available..." step will likely take some time while the ubuntu install process proceeds, packages are downloaded, etc. When you're getting started, and are rightly distrustful of things making forward progress, you may want to watch. The VirtualBox build can be monitored through the VirtualBox GUI iteself, and VMWare builds can be monitored via VNC, the connection details of which should be visible in your shell.

### Vagrant

#### Add the packer output to Vagrant

Tell Vagrant about the new box(es) we just built with `vagrant box add`

```bash
vagrant box add --name=virtualbox-ubuntu1404 file://builds/virtualbox-ubuntu1404.box
```

#### Install any necessary providers

Depending on your current setup, and the provider you intend to use, you may need ot add the appropriate plugins to support those providers.

##### VirtualBox Provider

Vagrant supports VirtualBox by default. You will of course need to [download VirtualBox](https://www.virtualbox.org/wiki/Downloads)

##### VMWare Fusion Provider Plugin (should you need it)

*Note that the vagrant-vmware-fusion plugin is not free and requires a license.*

[Installation and license docuementation](http://docs.vagrantup.com/v2/vmware/installation.html)


#### Init and Launch the VM

```bash
vagrant init
```

The previous command will create a Vagrantfile which defines the parameters for the VM we will launch in the next step. Refer to the [Vagrantfile documentation](http://docs.vagrantup.com/v2/vagrantfile/index.html) for details, but in general you may be interested in increasing the resources allocated to your VM.


## License

MIT license.

## Author Information

Created in 2014 by [Jeff Geerling](http://jeffgeerling.com/), author of [Ansible for DevOps](http://ansiblefordevops.com/).
