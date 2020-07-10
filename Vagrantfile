# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  
  config.vm.box = "centos/7"
  config.vm.box_version="2004.01"
  
  config.vm.network "private_network", ip: "192.168.33.11"

  config.vm.synced_folder ".", "/vagrant", disable :true

  
  config.vm.provision "shell", inline: <<-SHELL
# installing postgres   
   sudo yum -y install postgresql-server 

# configure postgres  
   sudo su -c "initdb" - postgres
   echo "host\tall\tall192.168.33.11/32\tmdS" | sudo -u postgres tee /var/lib/pgsql/data/pg_hba.conf > /dev/null
   sudo su -c "sed -i \"s/#listen_addresses = 'localhost'/listen_addresses = '*'/g\" /var/lib/pgsql/data/postgresql.conf" - postgres
   sudo su -c "pg_ctl start" - postgres
   sudo su -c "psql -c \"CREATE USER metabase WITH PASSWORD 'password';\"" - postgres
   sudo su -c "psql -c \"CREATE DATABASE metabasedb WITH OWNER metabase;\"" - postgres
   
# configure firewall to allowed postgres
   sudo systemctl enable firewalld
   sudo systemctl start firewalld
   sudo firewall-cmd --zone=public --add-port=5432/tcp --permanent
   sudo firewall-cmd --reload
  SHELL
end
