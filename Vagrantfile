# -*- mode: ruby -*-
# vi: set ft=ruby ts=2 sw=4 :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "trusty32"
  config.vm.box_url = "https://cloud-images.ubuntu.com/vagrant/trusty/current/trusty-server-cloudimg-i386-vagrant-disk1.box"

  # Use NAT'd networking
  config.vm.network :forwarded_port, guest: 80, host: 8808
  # config.vm.network :forwarded_port, guest: 443, host: 4443

  # This bridges the adapter, the VM will appear as real machine on your network
  # config.vm.network "public_network"

  # Shell bad, chef good!  Put any custom shell config in this file
  # config.vm.provision :shell, :path => "provision.sh"

  config.vm.provider :virtualbox do |vb|
    vb.customize ["modifyvm", :id, "--memory", "2048"]
  end

  # change our mounted folder - only indlude this for development
  # if you are deploying you MUST comment this line out or the 
  # site will deploy to the host machine
  config.vm.synced_folder "html/", "/var/www/html"

  # This folder is excluded from the git check in (it's a public
  # repo).  Here we look for id_rsa and id_rsa.pub to install so
  # we can access private repos for deployment
  config.vm.synced_folder "keys/", "/tmp/keys"

  config.vm.provision "chef_solo" do |chef|
    chef.log_level = :debug

    chef.add_recipe "apt"
    chef.add_recipe "build-essential"
    chef.add_recipe "git"
    chef.add_recipe "vim"
    chef.add_recipe "apache2"
    chef.add_recipe "mysql::client"
    chef.add_recipe "mysql::server"
    chef.add_recipe "php"
    chef.add_recipe "apache2::mod_php5";
    chef.add_recipe "ssh_known_hosts"
    chef.add_recipe "composer"
    chef.add_recipe "ntdrush"
    chef.add_recipe "neondc"
    chef.add_recipe "drupal"

    chef.json = {
      "drupal" => {
        "manifest" => "http://192.168.21.95/manifest/vanillafudge.manifest",
        "dburl" => "mysql://drupal:drupal@host/drupal",
        "adminname" => "superadmin",
        "adminpass" => "ilikerandompasswords",
        "workingcopy" => true,
      },
      "mysql" => {
        "server_root_password" => "ilikerandompasswords",
        "server_debian_password" => "postinstallscriptsarestupid",
        "allow_remote_root" => false,
        "remove_anonymous_users" => true
      },
      "ssh_known_hosts" => {
          "known_hosts" => {
              "fqdn" => "github.com",
              "rsa" => "github.com,192.30.252.128 ssh-rsa AAAAB3NzaC1yc2EAAAABIwAAAQEAq2A7hRGmdnm9tUDbO9IDSwBK6TbQa+PXYPCPy6rbTrTtw7PHkccKrpp0yVhp5HdEIcKr6pLlVDBfOLX9QUsyCOV0wzfjIJNlGEYsdlLJizHhbn2mUjvSAHQqZETYP81eFzLQNnPHt4EVVUh7VfDESU84KezmD5QlWpXLmvU31/yMf+Se8xhHTvKSCZIFImWwoG6mbUoWf9nzpIoaSjB+weqqUUmpaaasXVal72J+UX2B+2RPW3RcT0eOzQgqlJL3RKrTJvdsjE3JEAvGq3lGHSZXy28G3skua2SmVi/w4yCE6gbODqnTWlg7+wC604ydGXA8VJiS5ap43JXiUFFAaQ=="
          }
      }
    }

  end

end
