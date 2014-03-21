#Setting up the environment

Everything is done in a [VirtualBox](https://www.virtualbox.org/) VM which can be started and configured with [vagrant](http://www.vagrantup.com/).

First create a 32bit CentOS base box for vagrant ([read on the hows and whys](http://www.ampelofilosofies.gr/software/2014/03/15/building-with-rake-II)), then go register with [KPIT tools](http://www.kpitgnutools.com/index.php) and download the Linux RPM for the RX CPU family. 

Edit the [Vagrantfile](https://github.com/damphyr/embedded-C-rake/blob/master/Vagrantfile) with the correct name for the RPM (assuming you saved it in the setup/ folder) because by the time you read this there will probably be a new version out (or look for the 14.01 version to be on the safe side)

The tool setup described in [RubyOnCentOS.md](RubyOnCentOS.md) is done using the vagrant shell provisioner and the script is embedded in the Vagrantfile. There are just a couple extra lines added to the Ruby setup that install the KPIT toolchain:

```bash
#vagrant is configured to mount the repository root in the VM
cd /home/vagrant/host_share
rpm --install gnurx_v14.01_elf-1-1.i386.rpm
#add the toolchain to the PATH
cp bash_profile /home/vagrant/.bash_profile
chown vagrant:vagrant /home/vagrant/.bash_profile
chmod 544 /home/vagrant/.bash_profile
```