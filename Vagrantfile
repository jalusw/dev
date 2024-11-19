Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64" 
  
  config.vm.network "private_network", type: "dhcp"

  # config.vm.network "public_network"

  config.vm.provision "shell", inline: <<-SHELL
    # Update and upgrade the system
    apt-get update && apt-get upgrade -y

    # Install common tools and libraries
    apt-get install -y build-essential curl git software-properties-common apt-transport-https

    # Install Python
    apt-get install -y python3 python3-pip python3-apt

    # Install Node.js
    curl -fsSL https://deb.nodesource.com/setup_18.x | bash -
    apt-get install -y nodejs

    # Install Go
    wget https://go.dev/dl/go1.21.1.linux-amd64.tar.gz
    tar -C /usr/local -xzf go1.21.1.linux-amd64.tar.gz
    echo "export PATH=\$PATH:/usr/local/go/bin" >> /etc/profile
    source /etc/profile

    # Install Ruby and Rails
    apt-get install -y gnupg2
    curl -sSL https://rvm.io/mpapis.asc | gpg --import -
    curl -sSL https://rvm.io/pkuczynski.asc | gpg --import -
    curl -sSL https://get.rvm.io | bash -s stable
    source /etc/profile.d/rvm.sh
    rvm install 3.2.2
    rvm use 3.2.2 --default
    gem install rails

    # Install PostgreSQL
    apt-get install -y postgresql postgresql-contrib

    # Install MongoDB
    curl -fsSL https://www.mongodb.org/static/pgp/server-6.0.asc | apt-key add -
    echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu focal/mongodb-org/6.0 multiverse" | tee /etc/apt/sources.list.d/mongodb-org-6.0.list
    apt-get update
    apt-get install -y mongodb-org
    systemctl start mongod
    systemctl enable mongod

    # Install MySQL
    apt-get install -y mysql-server
    systemctl start mysql
    systemctl enable mysql

    # Clean up
    apt-get autoremove -y
    apt-get clean
  SHELL

  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4096"
    vb.cpus = 2
  end
end
