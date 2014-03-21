#Installing Ruby from source on CentOS

Ruby 2.1.1 on CentOS 6.5.

Using the vagrant base box created for the [32bit CentOS 6.5](CentOSBaseBox.md) you need the following commands to install Ruby from source:

```bash
sudo yum install zlib zlib-devel
sudo yum install openssl-devel
sudo yum install libyaml libyaml-devel
sudo yum install wget
sudo wget http://cache.ruby-lang.org/pub/ruby/2.1/ruby-2.1.1.tar.gz
sudo tar xvf ruby-2.1.1.tar.gz
sudo ./configure
sudo make
sudo make install
#at this point we need to reload the terminal environment for everything to work.
#the neatest way to do this is:
#CAUTION, this will only work if you configure the system to allow passwordless sudo for the vagrant user
exec su
gem update --system
gem i bundler
```