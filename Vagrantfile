Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu 20.04"
  config.vm.network "forwarded_port", guest: 5432, host: 5432, host_ip: "127.0.0.1" 
  config.ssh.insert_key = false
  config.vm.provision "shell", inline: <<-SHELL
    echo "Create the file repository configuration:"
    sudo sh -c 'echo "deb [arch=amd64]  http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'
    echo "Import the repository signing key:"
    wget --quiet -O - https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo apt-key add -  
    echo "Updating system"
    apt-get update
#    apt-get dist-upgrade -y
    echo "Installing postgres"
    sudo DEBIAN_FRONTEND=noninteractive apt-get install -yq postgresql-8.4
    echo "Configuring and restarting PostgreSQL"
    echo 'listen_addresses = '"'"'*'"'" >> /etc/postgresql/8.4/main/postgresql.conf
    echo 'host    all             all             0.0.0.0/0            md5' >> /etc/postgresql/8.4/main/pg_hba.conf
    systemctl restart postgresql
    echo "Creating vagrant user and database"
    echo "CREATE ROLE vagrant CREATEDB CREATEROLE CREATEUSER LOGIN UNENCRYPTED PASSWORD 'vagrant'" | sudo -u postgres psql -a -f -
    echo "CREATE DATABASE vagrant OWNER vagrant" | sudo -u postgres psql -a -f -
  SHELL
end
