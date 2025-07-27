# Debian9 Packer Box

Automated build of a Vagrant Box with Debian 9, Docker, and nginx using Packer and VirtualBox.

## Description

This project creates a ready-to-use Vagrant Box for development and testing.  
Debian 9 is installed automatically via preseed, then Docker and nginx are installed via a shell script.

## Project Structure

- `debian9-config.json` — main Packer configuration file
- `http/preseed.cfg` — preseed file for automated Debian installation
- `scripts/docker-nginx.sh` — script for installing Docker and nginx

## Requirements

- [Packer](https://www.packer.io/downloads)
- [VirtualBox](https://www.virtualbox.org/)
- [Vagrant](https://www.vagrantup.com/)
- Debian 9 ISO image (downloaded automatically or place in `iso-cd/`)

## Build the Image

```sh
packer build debian9-config.json
```

## Use with Vagrant

1. Add the box:
   ```sh
   vagrant box add debian9-pack ./packer-debian-9-amd64_virtualbox.box
   ```
2. Create a `Vagrantfile`:
   ```ruby
   Vagrant.configure("2") do |config|
     config.vm.box = "debian9-pack"
     config.vm.hostname = "debian9"
     config.vm.network "private_network", type: "dhcp"
     config.vm.provider "virtualbox" do |vb|
       vb.memory = "1024"
       vb.cpus = 1
     end
   end
   ```
3. Start the VM:
   ```sh
   vagrant up
   ```

## Specs:

The virtual machine will be created with the following parameters:

OS: Debian 9 (installed via preseed)
Memory: 1024 MB (1 GB)
CPU: 1 core
Disk: 20 GB (20480 MB)
Network: private network (DHCP)
Virtual machine name: package-debian-9-amd64
User: vagrant (password: vagrant)
Installed software: Docker and nginx (automatically via script)
SSH: port 22, vagrant user
Provider: VirtualBox

All parameters are specified in the debian9-config.json and Vagrantfile files.
