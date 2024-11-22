# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  config.vm.box = "debian/bookworm64"

  config.vm.define "webSamu" do |webSamu|
    webSamu.vm.network "private_network", ip: "192.168.57.10"

    webSamu.vm.provision "shell", inline: <<-SHELL
    sudo apt update
    sudo apt install -y git
    sudo apt install -y nginx

    sudo mkdir -p /var/www/webSamu/html
    git clone https://github.com/cloudacademy/static-website-example /var/www/webSamu/html
    
    sudo chown -R www-data:www-data /var/www/webSamu/html #Indicar dueño y grupo
    sudo chmod -R 755 /var/www/webSamu #Gestionar permisos

    sudo cp /vagrant/webSamu /etc/nginx/sites-available/webSamu

    sudo ln -sf /etc/nginx/sites-available/webSamu /etc/nginx/sites-enabled/ #Crear enlace duro al archivo
    sudo nginx -t #iniciar nginx
    sudo systemctl restart nginx

  SHELL
  end 

end