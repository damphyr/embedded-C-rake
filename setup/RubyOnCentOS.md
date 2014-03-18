#Installing Ruby on CentOS

Package managers all across *ix distributions are a mess. Specifically for Ruby the versions available are usually about a month away from end-of-life or at best a couple of years behind the current stable. 

Even more specific, yum lists a 1.8.7 version that is long dead and consigned to the history books. So installation from source it is. For the purposes of this experiment this is more than adequate. And to make matters more interesting, lets get the latest and greatest at the time of writing, 2.1.1.

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
exec su
gem update --system
gem i bundler
```