#Create a vagrant base box for CentOS 6.5

This set of instructions applies to the minimal CentOS 6.5 32bit version in a VirtualBox VM.

##Initial

Get the ISO from http://isoredirect.centos.org/centos/6/isos/i386/  

Install in a new virtual machine (name the machine 'vagrant-centos65-minimal-i386'). 

Since this is going to be a vagrant base box the root password is 'vagrant'

Add a vagrant user, give him sudo rights and add the vagrant SSH key for passwordless login. From a root shell.

```bash
  useradd vagrant
  passwd vagrant
  groupadd admin
  usermod -G admin vagrant
  mkdir /home/vagrant/.ssh
  curl -k https://raw.github.com/mitchellh/vagrant/master/keys/vagrant.pub > /home/vagrant/.ssh/authorized_keys
  chown -R vagrant:users /home/vagrant/.ssh
  chmod 0700 /home/vagrant/.ssh
  chmod 0600 /home/vagrant/.ssh/authorized_keys
```

To allow the vagrant user to sudo without entering a password:

 * Change the sudoers file, from a root shell on the vm (with visudo)
 * Add SSH_AUTH_SOCK to the env_keep option
 * Comment out the Defaults requiretty line
 * Add the line %admin ALL=NOPASSWD: ALL

##Activate networking

In CentOS the network interfaces are not activated at boot time.

Edit /etc/sysconfig/network-scripts/ifcfg-eth0 and change the following:

```bash
ONBOOT=yes
NM_CONTROLLED=yes
BOOTPROTO=dhcp
```

##Cofigure OpenSSH

From a root shell.
```bash
yum -y install openssh-server openssh-clients
service sshd start
chkconfig sshd on
```

##Install rpmforge

Needed for the VirtualBox guest additions. From a root shell.
```bash
rpm --import http://apt.sw.be/RPM-GPG-KEY.dag.txt
rpm -K http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.i686.rpm
rpm -ihv http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.3-1.el6.rf.i686.rpm
```

## VirtualBox guest additions

From a root shell.
```bash
yum --enablerepo rpmforge install dkms
```
If DKMS is not used the Guest Additions will need to be reinstalled after every kernel update.

If the development environment and kernel source are not already installed. From a root shell:
```bash
yum groupinstall "Development Tools"
yum install kernel-devel
```
Reboot after installing the development packages, occasionally the build process fails to locate the correct headers. After rebooting follow the VirtualBox Guest Additions [installation guide](http://wiki.centos.org/HowTos/Virtualization/VirtualBox/CentOSguest).

##Setup port forwarding  

Shutdown the virtual machine and run 
```bash
VBoxManage modifyvm "vagrant-centos65-minimal-i386" --natpf1 "guestssh,tcp,,2222,,22"
```
##Install Chef

##Clean up

From a root shell:
```bash
yum clean all
```
Shutdown the VM

##Package the base box

```bash
`vagrant package --output centos65-minimal-i386.box --base vagrant-centos65-minimal-i386
``
