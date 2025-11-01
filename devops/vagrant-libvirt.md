# Complete Guide: Using Vagrant with libvirt on Fedora

This guide walks you through setting up and using Vagrant with KVM/QEMU/libvirt on Fedora Linux for high-performance virtual machines.

## Table of Contents

1. [What is Vagrant with libvirt?](#what-is-vagrant-with-libvirt)
2. [Prerequisites](#prerequisites)
3. [Installation](#installation)
4. [Initial Setup](#initial-setup)
5. [Creating Your First VM](#creating-your-first-vm)
6. [Managing VMs](#managing-vms)
7. [Common Operations](#common-operations)
8. [Troubleshooting](#troubleshooting)
9. [Best Practices](#best-practices)

## What is Vagrant with libvirt?

**Vagrant** is a tool for building and managing virtual machine environments. **libvirt** (with KVM/QEMU) is a native Linux virtualization solution that offers superior performance compared to VirtualBox.

**Benefits:**
- Native Linux kernel virtualization (KVM)
- Better performance and lower overhead
- Official Fedora/RHEL support
- Hardware-accelerated virtualization
- Better I/O and network performance

## Prerequisites

Before starting, ensure you have:
- Fedora Linux (or RHEL-based distribution)
- CPU with virtualization support (Intel VT-x or AMD-V)
- At least 4GB RAM (8GB+ recommended)
- Sudo/root access

### Verify CPU Virtualization Support

```bash
# Check if your CPU supports virtualization
egrep -c '(vmx|svm)' /proc/cpuinfo
```

If the output is greater than 0, your CPU supports virtualization.

## Installation

### Step 1: Install Virtualization Packages

```bash
# Install the virtualization group and Vagrant
sudo dnf install @virtualization vagrant

# Install development tools needed for the plugin
sudo dnf install -y gcc libvirt-devel make ruby-devel
```

### Step 2: Install vagrant-libvirt Plugin

```bash
# Install the vagrant-libvirt plugin
vagrant plugin install vagrant-libvirt

# Verify installation
vagrant plugin list
```

You should see `vagrant-libvirt` in the output.

### Step 3: Start and Enable libvirt Service

```bash
# Start the libvirt daemon
sudo systemctl enable --now libvirtd

# Verify it's running
sudo systemctl status libvirtd
```

### Step 4: Configure User Permissions

```bash
# Add your user to the libvirt group
sudo usermod -aG libvirt $USER

# Verify group membership
groups $USER
```

**Important:** Log out and log back in (or reboot) for group changes to take effect.

### Step 5: Verify Installation

```bash
# Check virsh command works without sudo
virsh list --all

# Check Vagrant recognizes libvirt provider
vagrant status
```

## Initial Setup

### Set libvirt as Default Provider (Optional)

Add this to your `~/.bashrc` or `~/.zshrc`:

```bash
export VAGRANT_DEFAULT_PROVIDER=libvirt
```

Then reload your shell:

```bash
source ~/.bashrc  # or source ~/.zshrc
```

### Configure libvirt Network (Optional)

Check the default network is active:

```bash
# List networks
virsh net-list --all

# Start default network if inactive
virsh net-start default
virsh net-autostart default
```

## Creating Your First VM

### Step 1: Create a Project Directory

```bash
mkdir -p ~/vagrant-projects/ubuntu-test
cd ~/vagrant-projects/ubuntu-test
```

### Step 2: Initialize a Vagrantfile

```bash
# Initialize with a basic Ubuntu box
vagrant init generic/ubuntu2204
```

This creates a basic `Vagrantfile` in your current directory.

### Step 3: Customize the Vagrantfile

Edit the `Vagrantfile`:

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.box = "generic/ubuntu2204"
  config.vm.hostname = "my-ubuntu-vm"
  
  config.vm.provider "libvirt" do |libvirt|
    libvirt.memory = 2048
    libvirt.cpus = 2
    libvirt.cpu_mode = "host-passthrough"
  end
  
  # Optional: provision with a shell script
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y vim curl git
    echo "VM provisioned successfully!"
  SHELL
end
```

### Step 4: Start the VM

```bash
# Download box and start VM
vagrant up

# Or explicitly specify provider
vagrant up --provider=libvirt
```

First run will download the box image (this may take a few minutes).

### Step 5: Connect to the VM

```bash
# SSH into the VM
vagrant ssh
```

You're now inside the virtual machine!

## Managing VMs

### Basic VM Lifecycle Commands

```bash
# Start/create VM
vagrant up

# Stop VM (graceful shutdown)
vagrant halt

# Restart VM
vagrant reload

# Suspend VM (save state)
vagrant suspend

# Resume suspended VM
vagrant resume

# Delete VM completely
vagrant destroy

# Delete without confirmation
vagrant destroy -f
```

### Checking VM Status

```bash
# Check status of current project's VM
vagrant status

# Check status of all Vagrant VMs
vagrant global-status

# Check with libvirt directly
virsh list --all
```

### SSH Access

```bash
# SSH into VM
vagrant ssh

# Run a single command
vagrant ssh -c "uname -a"

# Get SSH config details
vagrant ssh-config
```

### Provisioning

```bash
# Re-run provisioning scripts
vagrant provision

# Reload VM and provision
vagrant reload --provision
```

## Common Operations

### Port Forwarding

Add to your Vagrantfile:

```ruby
config.vm.network "forwarded_port", guest: 80, host: 8080
config.vm.network "forwarded_port", guest: 443, host: 8443
```

### Private Network

Add to your Vagrantfile:

```ruby
config.vm.network "private_network", ip: "192.168.121.10"
```

### Shared Folders

```ruby
# Using rsync (recommended for libvirt)
config.vm.synced_folder ".", "/vagrant", type: "rsync"

# Exclude patterns
config.vm.synced_folder ".", "/vagrant", 
  type: "rsync",
  rsync__exclude: [".git/", "node_modules/"]
```

### Snapshots

```bash
# Take a snapshot
vagrant snapshot save my-snapshot

# List snapshots
vagrant snapshot list

# Restore a snapshot
vagrant snapshot restore my-snapshot

# Delete a snapshot
vagrant snapshot delete my-snapshot
```

### Multiple VMs

Define multiple VMs in one Vagrantfile:

```ruby
Vagrant.configure("2") do |config|
  config.vm.define "web" do |web|
    web.vm.box = "generic/ubuntu2204"
    web.vm.hostname = "web-server"
    web.vm.network "private_network", ip: "192.168.121.10"
  end
  
  config.vm.define "db" do |db|
    db.vm.box = "generic/ubuntu2204"
    db.vm.hostname = "database"
    db.vm.network "private_network", ip: "192.168.121.11"
  end
end
```

Manage specific VMs:

```bash
vagrant up web
vagrant ssh web
vagrant halt db
```

### Box Management

```bash
# List installed boxes
vagrant box list

# Add a box manually
vagrant box add generic/ubuntu2204

# Update a box
vagrant box update

# Remove a box
vagrant box remove generic/ubuntu2204

# Remove old box versions
vagrant box prune
```

## Troubleshooting

### VM Won't Start

```bash
# Check libvirt service
sudo systemctl status libvirtd

# Check for permission issues
groups | grep libvirt

# View detailed logs
VAGRANT_LOG=debug vagrant up
```

### Network Issues

```bash
# Restart libvirt network
virsh net-destroy default
virsh net-start default

# Check network status
virsh net-list --all
```

### "No provider" Error

```bash
# Verify plugin is installed
vagrant plugin list

# Reinstall if needed
vagrant plugin uninstall vagrant-libvirt
vagrant plugin install vagrant-libvirt
```

### Performance Issues

Ensure CPU mode is set to host-passthrough:

```ruby
config.vm.provider "libvirt" do |libvirt|
  libvirt.cpu_mode = "host-passthrough"
  libvirt.disk_bus = "virtio"
  libvirt.nic_model_type = "virtio"
end
```

### Clean Up Stale VMs

```bash
# Remove stale entries
vagrant global-status --prune

# Force remove with virsh
virsh list --all
virsh undefine <vm-name>
```

### Plugin Installation Fails

Install additional dependencies:

```bash
sudo dnf install -y \
  gcc \
  libvirt-devel \
  libxml2-devel \
  libxslt-devel \
  make \
  ruby-devel \
  krb5-devel

vagrant plugin install vagrant-libvirt
```

## Best Practices

### 1. Use Version Control

Always commit your Vagrantfile to git:

```bash
git init
git add Vagrantfile
git commit -m "Initial Vagrantfile"
```

### 2. Set Resource Limits

Don't over-allocate resources:

```ruby
config.vm.provider "libvirt" do |libvirt|
  # Leave resources for host system
  libvirt.memory = 2048  # Not all your RAM
  libvirt.cpus = 2       # Not all your cores
end
```

### 3. Use Provisioning Scripts

Keep setup reproducible:

```ruby
config.vm.provision "shell", path: "setup.sh"
```

### 4. Document Your Setup

Add comments to your Vagrantfile explaining configuration choices.

### 5. Regular Cleanup

```bash
# Remove unused boxes
vagrant box prune

# Clean up old VMs
vagrant global-status --prune
```

### 6. Use Snapshots

Take snapshots before major changes:

```bash
vagrant snapshot save clean-state
# Make changes...
# If things break:
vagrant snapshot restore clean-state
```

### 7. Pin Box Versions

For reproducibility:

```ruby
config.vm.box = "generic/ubuntu2204"
config.vm.box_version = "4.3.12"
```

## Useful Resources

- [Vagrant Documentation](https://www.vagrantup.com/docs)
- [vagrant-libvirt GitHub](https://github.com/vagrant-libvirt/vagrant-libvirt)
- [Vagrant Box Catalog](https://app.vagrantup.com/boxes/search)
- [libvirt Documentation](https://libvirt.org/docs.html)

## Quick Reference

```bash
# Essential commands
vagrant up              # Start VM
vagrant halt            # Stop VM
vagrant ssh             # Connect to VM
vagrant destroy         # Delete VM
vagrant status          # Check status
vagrant reload          # Restart VM

# Box management
vagrant box list        # List boxes
vagrant box add <box>   # Download box
vagrant box remove      # Delete box

# Provisioning
vagrant provision       # Run provisioners
vagrant reload --provision  # Restart and provision

# Snapshots
vagrant snapshot save <name>     # Create snapshot
vagrant snapshot restore <name>  # Restore snapshot
vagrant snapshot list            # List snapshots

# Debugging
VAGRANT_LOG=debug vagrant up     # Verbose output
```

---

**Congratulations!** You now know how to use Vagrant with libvirt on Fedora. Start experimenting with different boxes and configurations to build your perfect development environment.
