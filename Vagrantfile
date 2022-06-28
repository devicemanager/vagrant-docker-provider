VAGRANTFILE_API_VERSION = "2"
ENV["LC_ALL"] = "en_US.UTF-8"

CENTOS_CLUSTER = 3
UBUNTU_CLUSTER = 1

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  1.upto(UBUNTU_CLUSTER) do |i|
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

  1.upto(CENTOS_CLUSTER) do |i|
    config.vm.define vm_name = "centos#{i}" do |config|
      config.vm.provider "docker" do |docker|
        docker.image = "devicemanager/vagrant-provider:centos"
        docker.has_ssh = true
        docker.remains_running = true
        docker.privileged = true
        docker.volumes = ["/sys/fs/cgroup:/sys/fs/cgroup:rw"]
        docker.create_args = ["--cgroupns=host"]
      end
      config.vm.hostname = vm_name
      config.vm.network :private_network, ip: '192.168.77.' + (20 + i).to_s
    end
  end

  ubuntu_script = <<-SCRIPT
      case "$HOSTNAME" in
        node1)
           echo This is node1
           apt-get -y update
           apt-get -y install ansible
           ;;
        *)
          echo This node has not been defined
      esac
  SCRIPT

  centos_script = <<-SCRIPT
      case "$HOSTNAME" in
        centos1)
           echo This is centos1
           yum -y update
           ;;
        centos2) 
          echo This is centos2
           yum -y update
          ;;
        centos3)
          echo This is centos3
           yum -y update
          ;;
        *)
          echo This node has not been defined
      esac
  SCRIPT

  ubuntu_script.sub! 'UBUNTU_CLUSTER', UBUNTU_CLUSTER.to_s
  i=1
  config.vm.provision "shell", inline: ubuntu_script

  centos_script.sub! 'CENTOS_CLUSTER', CENTOS_CLUSTER.to_s
  i=1
  config.vm.provision "shell", inline: centos_script

end
