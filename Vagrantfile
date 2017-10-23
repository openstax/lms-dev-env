hostname = "lms-dev-env"

vm_memory = 4096
vm_cpus = 2

forwarded_ports = [
  { guest: 2999, host: 2999 }, # Accounts
  { guest: 3000, host: 3000 }, # Canvas
  { guest: 3001, host: 3001 }, # Tutor
]

# Run Canvas with:
# source ~/anyenv.sh && bundle exec rails server -b 0.0.0.0

Vagrant.configure(2) do |config|
  config.vm.box = "trusty64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.provision :ansible_local do |ansible|
    ansible.tags = ENV["ANSIBLE_TAGS"] if ENV["ANSIBLE_TAGS"]
    ansible.playbook = "main.yml"
  end

  config.vm.hostname = hostname

  forwarded_ports.each do |forwarded_port|
    config.vm.network :forwarded_port, guest: forwarded_port[:guest], host: forwarded_port[:host]
  end

  config.vm.provider "virtualbox" do |vb|
    vb.memory = vm_memory
    vb.cpus = vm_cpus

    # Use this to use normal DNS (good for installing stuff)
    #
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]

    # Use nat dns to avoid bizarre dns slowdowns (after installing normally)
    #
    # vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    # vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
  end
end
