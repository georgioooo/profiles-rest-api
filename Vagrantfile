# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrant configuration file

Vagrant.configure("2") do |config|

  # -------------------------------
  # 1. Specify the Ubuntu Box
  # -------------------------------

  # Using Ubuntu 22.04 LTS (Jammy Jellyfish) for better support and updates
  config.vm.box = "ubuntu/jammy64"

  # Specify the latest box version available
  # As of the knowledge cutoff in October 2023, replace with the latest if available
  config.vm.box_version = "20241002.0.0"  # Example version, update as needed

  # -------------------------------
  # 2. Configure the VirtualBox Provider
  # -------------------------------

  config.vm.provider "virtualbox" do |vb|
    # Allocate sufficient memory and CPU resources
    vb.memory = "2048"     # 2 GB RAM
    vb.cpus = 2            # 2 CPU cores

    # Enable GUI to observe the VM's boot process (optional, useful for debugging)
    vb.gui = false         # Set to true if you want to see the VM's graphical interface

    # Set the VM name for easier identification
    vb.name = "my_ubuntu_vm"

    # Optional: Adjust the VM's network settings further if needed
    # vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    # vb.customize ["modifyvm", :id, "--nictype1", "virtio"]
  end

  # -------------------------------
  # 3. Network Configuration
  # -------------------------------

  # Forward guest port 8000 to host port 8000
  config.vm.network "forwarded_port", guest: 8000, host: 8000, auto_correct: true

  # Ensure SSH port is correctly forwarded (default is 22 -> 2222)
  config.vm.network "forwarded_port", guest: 22, host: 2222, id: "ssh", auto_correct: true

  # Optional: Add a private network with a static IP
  # config.vm.network "private_network", ip: "192.168.50.4"

  # -------------------------------
  # 4. Provider-Specific Configurations
  # -------------------------------

  # Increase boot timeout to allow ample time for the VM to initialize
  config.vm.boot_timeout = 600  # 10 minutes

  # -------------------------------
  # 5. Provisioning Scripts
  # -------------------------------

  config.vm.provision "shell", inline: <<-SHELL
    # Disable automatic daily apt updates to prevent delays
    sudo systemctl disable apt-daily.service
    sudo systemctl disable apt-daily.timer
    sudo systemctl disable apt-daily-upgrade.service
    sudo systemctl disable apt-daily-upgrade.timer

    # Update package lists
    sudo apt-get update -y

    # Install necessary packages
    sudo apt-get install -y python3-venv zip

    # Set up bash aliases for the vagrant user
    sudo -u vagrant bash -c 'touch /home/vagrant/.bash_aliases'
    sudo -u vagrant bash -c 'grep -q PYTHON_ALIAS_ADDED /home/vagrant/.bash_aliases || echo -e "# PYTHON_ALIAS_ADDED\nalias python="python3"" >> /home/vagrant/.bash_aliases'
  SHELL

  # -------------------------------
  # 6. Synced Folders (Optional)
  # -------------------------------

  # Uncomment and modify the following lines if you want to sync a folder from the host to the guest
  # config.vm.synced_folder "./data", "/vagrant_data", type: "virtualbox"

  # -------------------------------
  # 7. Additional Configurations (Optional)
  # -------------------------------

  # You can add more configurations below as needed
  # For example, configuring shell environment variables, additional provisioning steps, etc.

end
