# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  ############################################################
  # Provider for Docker on Intel or ARM (aarch64)
  ############################################################
  config.vm.hostname = "centos"
  config.vm.provider :docker do |docker, override|
    override.vm.box = nil
    docker.image = "devicemanager/vagrant-provider:centos"
    docker.remains_running = true
    docker.has_ssh = true
    docker.privileged = true
    docker.volumes = ["/sys/fs/cgroup:/sys/fs/cgroup:rw"]
    docker.create_args = ["--cgroupns=host"]
  end  

  # Install Docker and pull an image
  # config.vm.provision :docker do |d|
  #   d.pull_images "alpine:latest"
  # end

end
