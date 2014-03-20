# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

provision_script=<<-EOT
yum -y install zlib zlib-devel
yum -y install openssl-devel
yum -y install libyaml libyaml-devel
yum -y install wget
wget http://cache.ruby-lang.org/pub/ruby/2.1/ruby-2.1.1.tar.gz
tar xvf ruby-2.1.1.tar.gz
cd ruby-2.1.1
./configure
make
make install
echo Setting up environment
cp /home/vagrant/host_share/setup/bash_profile /home/vagrant/.bash_profile
chown vagrant:vagrant /home/vagrant/.bash_profile
chmod 544 /home/vagrant/.bash_profile
rpm --install /home/vagrant/host_share/setup/gnurx_v14.01_elf-1-1.i386.rpm
exec su
gem update --system
gem i bundler
EOT

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.provision "shell", inline: provision_script
  config.vm.box = "centos65-minimal-i686.box"
  config.vm.box_url = "centos65-minimal-i686.box"
  config.vm.synced_folder ".", "/home/vagrant/host_share", :disabled => false
end