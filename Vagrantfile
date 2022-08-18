BRIDGE_NET="192.168.1."
INTERNAL_NET="192.168.1."

servers=[
  {
    :hostname => "ubuntubionic",
    :ip => BRIDGE_NET + "150",
    :ip_int => "1",
    :ram => 2000,
    :box => "ubuntu/bionic64",
    :hdd_name => "ubuntubionic_hdd.vdi",
    :hdd_size => "10000",
    :provision_script => "ubuntubionic.sh"
  },
  {
    :hostname => "centos7",
    :ip => BRIDGE_NET + "151",
    :ip_int => "2",
    :ram => 2000,
    :box => "centos/7",
    :hdd_name => "centos7_hdd.vdi",
    :hdd_size => "10000",
    :provision_script => "centos7.sh"
  }
]
 
Vagrant.configure(2) do |config|
    config.vm.synced_folder ".", "/vagrant", disabled: true
    servers.each do |machine|
        config.vm.define machine[:hostname] do |node|
            node.vm.box = machine[:box]
            node.vm.usable_port_range = (2200..2250)
            node.vm.hostname = machine[:hostname]
            node.vm.network "public_network", ip: machine[:ip], bridge: 'wlp9s0'
            node.vm.provision :shell, path: machine[:provision_script]
            node.vm.provider "virtualbox" do |vb|
                vb.customize ["modifyvm", :id, "--memory", machine[:ram]]
                vb.name = machine[:hostname]
                if (!machine[:hdd_name].nil?)
                    unless File.exist?(machine[:hdd_name])
                        vb.customize ["createhd", "--filename", machine[:hdd_name], "--size", machine[:hdd_size]]
                    end
                    #vb.customize ["storageattach", :id, "--storagectl", "SATA", "--port", 1, "--device", 0, "--type", "hdd", "--medium", machine[:hdd_name]]
                end

            end
        end
    end
end