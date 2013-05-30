# -*- mode: ruby -*-
# vi: set ft=ruby :
VAGRANT_API_VERSION = "2"

Vagrant.configure(VAGRANT_API_VERSION) do |config|
  # bumping memory and cpu to improve compilation speed (not sure if compiler forks though)
  config.vm.provider :virtualbox do |v|
      v.customize ["modifyvm", :id, "--cpus", "2"]
      v.customize ["modifyvm", :id, "--memory", "1024"]
  end

  config.vm.define :centos6_4 do |centos6_4|
    BOX_NAME             = "centos-64-x64-vbox4210-nocm"
    centos6_4.vm.box     = BOX_NAME
    centos6_4.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/#{BOX_NAME}.box"

    centos6_4_provisioning_script = <<SCRIPT
wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
wget http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
rpm -Uvh remi-release-6*.rpm epel-release-6*.rpm 

yum install -y rpm-build rpmdevtools gdbm-devel tcl-devel db4-devel \
               libyaml-devel byacc libyaml libffi-devel
rpmdev-setuptree
cd ~/rpmbuild/SOURCES
wget http://ftp.ruby-lang.org/pub/ruby/1.9/ruby-1.9.3-p429.tar.gz
cd ~/rpmbuild/SPECS
curl https://raw.github.com/cv/ruby-1.9.3-rpm/master/ruby193.spec > ruby193.spec
rpmbuild -bb ruby193.spec
rpm -Uvh ~/rpmbuild/RPMS/x86_64/ruby-1.9.3p429-2.el6.x86_64.rpm
SCRIPT

    centos6_4.vm.provision :shell, :inline => centos6_4_provisioning_script
  end
end

