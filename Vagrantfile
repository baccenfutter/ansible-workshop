# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.provision "file", source: "~/.ssh/id_rsa.pub", destination: "~/.ssh/id_rsa.pub"
  config.vm.provision "shell", inline: "cat ~vagrant/.ssh/id_rsa.pub >> ~vagrant/.ssh/authorized_keys"

  config.vm.define "db" do |db|
    db.vm.box = "bento/debian-9.0"
    db.vm.network "private_network", ip: "10.42.0.2"
    db.vm.synced_folder ".", "/vagrant"
    db.vm.provision "shell" do |s|
      s.inline = <<-SHELL
        apt-get update
        apt-get install -y mysql-server
        mysql -Bse "CREATE USER 'ansible'@'%' IDENTIFIED BY 'ansible';"
        mysql -Bse "GRANT SELECT ON *.* TO ansible@'%';"
        mysql -Bse "CREATE USER 'openexchange'@'%' IDENTIFIED BY 'laekio8aid6teeTu';"
        mysql -Bse "GRANT ALL PRIVILEGES ON *.* TO openexchange@'%';
        sed -e 's/127\.0\.0\.1/0\.0\.0\.0/' -i /etc/mysql/mariadb.conf.d/50-server.cnf
      SHELL
    end
  end

  config.vm.define "app" do |app|
    app.vm.box = "bento/debian-9.0"
    app.vm.network "private_network", ip: "10.42.0.3"
    app.vm.synced_folder ".", "/vagrant"
  end
end
