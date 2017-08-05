# canvas_config = {
#   servers: {
#     canvas: {
#       memory: 4096,
#       cpus: 4,
#       forwarded_ports: [ { guest: 3000, host: 3000 } ],
#     }
#   },
# }

hostname = "lms-dev-env"

vm_memory = 4096
vm_cpus = 2

forwarded_ports = [
  { guest: 2999, host: 2999 }, # Accounts
  { guest: 3000, host: 3000 }, # Canvas
  { guest: 3001, host: 3001 }, # Tutor
]

# source ~/anyenv.sh && bundle exec rails server -b 0.0.0.0

Vagrant.configure(2) do |config|
  config.vm.box = "trusty64"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-amd64-vagrant-disk1.box"

  config.vm.provision :ansible_local do |ansible|
    ansible.tags = ENV["ANSIBLE_TAGS"] if ENV["ANSIBLE_TAGS"]
    ansible.playbook = "playbooks/main.yml"
  end

  config.vm.hostname = hostname

  forwarded_ports.each do |forwarded_port|
    config.vm.network :forwarded_port, guest: forwarded_port[:guest], host: forwarded_port[:host]
  end

  config.vm.provider "virtualbox" do |vb|
    vb.memory = vm_memory
    vb.cpus = vm_cpus

    # use nat dns to avoid bizarre dns slowdowns
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
  end


  # canvas_config[:servers].keys.each do |hostname|
  #   config.vm.define hostname do |m|
  #     m.vm.hostname = hostname
  #     if canvas_config[:servers][hostname][:forwarded_ports]
  #       canvas_config[:servers][hostname][:forwarded_ports].each do |forwarded_port|
  #         m.vm.network :forwarded_port, guest: forwarded_port[:guest], host: forwarded_port[:host]
  #       end
  #     end

  #     m.vm.provider "virtualbox" do |vb|
  #       vb.memory = canvas_config[:servers][hostname][:memory]
  #       vb.cpus = canvas_config[:servers][hostname][:cpus]

  #       # use nat dns to avoid bizarre dns slowdowns
  #       vb.customize ["modifyvm", :id, "--natdnsproxy1", "off"]
  #       vb.customize ["modifyvm", :id, "--natdnshostresolver1", "off"]
  #     end
  #   end
  # end
end
