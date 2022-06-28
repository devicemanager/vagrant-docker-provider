VAGRANTFILE_API_VERSION = "2"
ENV["LC_ALL"] = "en_US.UTF-8"

CLUSTER_SIZE = 3

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  1.upto(CLUSTER_SIZE) do |i|
    config.vm.define vm_name = "node#{i}" do |config|

      config.vm.provider "docker" do |docker|
        docker.image = "devicemanager/vagrant-provider:ubuntu"
        docker.has_ssh = true
        docker.remains_running = true
        docker.privileged = true
        docker.volumes = ["/sys/fs/cgroup:/sys/fs/cgroup:rw"]
        docker.create_args = ["--cgroupns=host"]
      end
      config.vm.hostname = vm_name
      config.vm.network :private_network, ip: '192.168.77.' + (10 + i).to_s
    end
  end

  script = <<-SCRIPT
      apt-get -y update
      case "$HOSTNAME" in
        node1)
           echo This is node1
           apt-get -y install openssh-server 
           ;;
        node2)
           echo This is node2
           ;;
        node3) 
          echo This is node3
          ;;
        node4)
          echo This is node4
          ;;
        *)
          echo This node has not been defined
      esac
  SCRIPT
  script.sub! 'CLUSTER_SIZE', CLUSTER_SIZE.to_s
  i=1
  config.vm.provision "shell", inline: script

end
