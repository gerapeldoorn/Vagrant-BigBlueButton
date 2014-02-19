# -*- mode: ruby -*-
# vi: set ft=ruby :

$script = <<SCRIPT
	export DEBIAN_FRONTEND=noninteractive
	echo "Settaggio locale"
	apt-get -y install language-pack-en
	update-locale LANG="en_US.UTF-8"
	sudo su
	echo "aggiunta del repository di BBB"
	wget http://ubuntu.bigbluebutton.org/bigbluebutton.asc -O- |  apt-key add -
	echo "deb http://ubuntu.bigbluebutton.org/lucid_dev_081/ bigbluebutton-lucid main" |  tee /etc/apt/sources.list.d/bigbluebutton.list
	# echo "aggiunta del repository multiverse"
	# echo "deb http://us.archive.ubuntu.com/ubuntu/ lucid multiverse" | tee -a /etc/apt/sources.list
	echo "Aggiornamento del sistema"
	apt-get -q update
	apt-get -y -q intall build-essential linux-kernel-headers
	apt-get -q -y dist-upgrade

	echo "Si comincia"
	echo "Installazione di Libreoffice"
	debconf-set-selections | msttcorefonts msttcorefonts/acccepted-mscorefonts-eula boolean true
	debconf-set-selections | msttcorefonts msttcorefonts/present-mscorefonts-eula boolean false
	wget http://bigbluebutton.googlecode.com/files/openoffice.org_1.0.4_all.deb
	dpkg -i openoffice.org_1.0.4_all.deb

	apt-get -y -q install python-software-properties

	apt-add-repository ppa:libreoffice/libreoffice-4-0
	apt-get -q update

	apt-get -y -q install libreoffice-common
	apt-get -y -q install libreoffice

	echo "installazione di ruby"
	wget https://bigbluebutton.googlecode.com/files/ruby1.9.2_1.9.2-p290-1_amd64.deb
	dpkg -i ruby1.9.2_1.9.2-p290-1_amd64.deb
 	update-alternatives --install /usr/bin/ruby ruby /usr/bin/ruby1.9.2 500 \
                         --slave /usr/bin/ri ri /usr/bin/ri1.9.2 \
                         --slave /usr/bin/irb irb /usr/bin/irb1.9.2 \
                         --slave /usr/bin/erb erb /usr/bin/erb1.9.2 \
                         --slave /usr/bin/rdoc rdoc /usr/bin/rdoc1.9.2
 update-alternatives --install /usr/bin/gem gem /usr/bin/gem1.9.2 500
 apt-get -y -q install -f

	echo "Installazione di ffmpeg"
 	apt-get install -y -q git-core checkinstall yasm texi2html libvorbis-dev libx11-dev libxfixes-dev zlib1g-dev pkg-config

	LIBVPX_VERSION=1.2.0
	FFMPEG_VERSION=2.0.1

	if [ ! -d "/usr/local/src/libvpx-${LIBVPX_VERSION}" ]; then
		cd /usr/local/src
		git clone http://git.chromium.org/webm/libvpx.git "libvpx-${LIBVPX_VERSION}"
		cd "libvpx-${LIBVPX_VERSION}"
		git checkout "v${LIBVPX_VERSION}"
		./configure
		make
		checkinstall --pkgname=libvpx --pkgversion="${LIBVPX_VERSION}" --backup=no --deldoc=yes --default
	fi

	if [ ! -d "/usr/local/src/ffmpeg-${FFMPEG_VERSION}" ]; then
		cd /usr/local/src
		wget "http://ffmpeg.org/releases/ffmpeg-${FFMPEG_VERSION}.tar.bz2"
		tar -xjf "ffmpeg-${FFMPEG_VERSION}.tar.bz2"
		cd "ffmpeg-${FFMPEG_VERSION}"
		./configure --enable-version3 --enable-postproc --enable-libvorbis --enable-libvpx
		make
		checkinstall --pkgname=ffmpeg --pkgversion="5:${FFMPEG_VERSION}" --backup=no --deldoc=yes --default
	fi

	echo "Installazione di BBB"
	apt-get -y -q install bigbluebutton
	apt-get -y -q install bbb-demo
	
	echo "Pulizia"
	apt-get -y -q autoremove
	apt-get -y clean
	echo "Avvio di BBB"
	bbb-conf --clean
	bbb-conf --check
	bbb-conf --setip 192.168.33.10
	exit
SCRIPT
# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # All Vagrant configuration is done here. The most common configuration
  # options are documented and commented below. For a complete reference,
  # please see the online documentation at vagrantup.com.

  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "lucid64"

  # The url from where the 'config.vm.box' box will be fetched if it
  # doesn't already exist on the user's system.
  config.vm.box_url = "http://files.vagrantup.com/lucid64.box"

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing "localhost:8080" will access port 80 on the guest machine.
  # config.vm.network :forwarded_port, guest: 80, host: 8080
  # config.vm.network :forwarded_port, guest: 9123, host: 9123
  # config.vm.network :forwarded_port, guest: 1935, host: 9135

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network :private_network, ip: "192.168.33.10"

  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network :public_network

  # If true, then any SSH connections made will enable agent forwarding.
  # Default value: false
  # config.ssh.forward_agent = true

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  # config.vm.synced_folder "../data", "/vagrant_data"

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
   config.vm.provider :virtualbox do |vb|
			vb.memory = 1024
  #   # Don't boot with headless mode
  #   vb.gui = true
  #
  #   # Use VBoxManage to customize the VM. For example to change memory:
  #   vb.customize ["modifyvm", :id, "--memory", "1024"]
   end
  #
  # View the documentation for the provider you're using for more
  # information on available options.

  # Enable provisioning with Puppet stand alone.  Puppet manifests
  # are contained in a directory path relative to this Vagrantfile.
  # You will need to create the manifests directory and a manifest in
  # the file base.pp in the manifests_path directory.
  #
  # An example Puppet manifest to provision the message of the day:
  #
  # # group { "puppet":
  # #   ensure => "present",
  # # }
  # #
  # # File { owner => 0, group => 0, mode => 0644 }
  # #
  # # file { '/etc/motd':
  # #   content => "Welcome to your Vagrant-built virtual machine!
  # #               Managed by Puppet.\n"
  # # }
  #
  # config.vm.provision :puppet do |puppet|
  #   puppet.manifests_path = "manifests"
  #   puppet.manifest_file  = "site.pp"
  # end

  # Enable provisioning with chef solo, specifying a cookbooks path, roles
  # path, and data_bags path (all relative to this Vagrantfile), and adding
  # some recipes and/or roles.
  #
  # config.vm.provision :chef_solo do |chef|
  #   chef.cookbooks_path = "../my-recipes/cookbooks"
  #   chef.roles_path = "../my-recipes/roles"
  #   chef.data_bags_path = "../my-recipes/data_bags"
  #   chef.add_recipe "mysql"
  #   chef.add_role "web"
  #
  #   # You may also specify custom JSON attributes:
  #   chef.json = { :mysql_password => "foo" }
  # end

  # Enable provisioning with chef server, specifying the chef server URL,
  # and the path to the validation key (relative to this Vagrantfile).
  #
  # The Opscode Platform uses HTTPS. Substitute your organization for
  # ORGNAME in the URL and validation key.
  #
  # If you have your own Chef Server, use the appropriate URL, which may be
  # HTTP instead of HTTPS depending on your configuration. Also change the
  # validation key to validation.pem.
  #
  # config.vm.provision :chef_client do |chef|
  #   chef.chef_server_url = "https://api.opscode.com/organizations/ORGNAME"
  #   chef.validation_key_path = "ORGNAME-validator.pem"
  # end
  #
  # If you're using the Opscode platform, your validator client is
  # ORGNAME-validator, replacing ORGNAME with your organization name.
  #
  # If you have your own Chef Server, the default validation client name is
  # chef-validator, unless you changed the configuration.
  #
  #   chef.validation_client_name = "ORGNAME-validator"
	config.vm.provision :shell, inline: $script
end
