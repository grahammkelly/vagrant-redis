VAGRANT_API_VER = "2"
# All Vagrant configuration is done below. The "2" above
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.

VM_NAME = "redis-test-server"
Vagrant.configure(VAGRANT_API_VER) do |config|
  config.vm.box = "ubuntu/trusty64"
  config.vm.hostname = VM_NAME

  config.vm.network :forwarded_port, guest: 6379, host: 6379

  # config.vm.network "private_network", ip: "192.168.33.14"
  # config.vm.network "public_network"

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  config.vm.synced_folder "#{Dir.pwd}/data", "/vagrant_data"

  config.vm.provider "virtualbox" do |vb|
  	vb.memory = "1024"
    vb.name = VM_NAME
  end

  config.vm.provision "shell" do |s|
    ssh_pub_key = File.readlines("#{Dir.home}/.ssh/id_rsa.pub").first.strip
    s.inline = <<-SHELL
      echo "Ensuring the English locale is available"
      sudo locale-gen en_IE.UTF-8

      # Create the SSH keys to allow direct SSH onto the box (not through vagrant)
      echo #{ssh_pub_key} >> /home/vagrant/.ssh/authorized_keys
      echo #{ssh_pub_key} >> /root/.ssh/authorized_keys
    SHELL
  end

  config.vm.provision :shell, :path => "init.sh"
end
