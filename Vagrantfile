# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"

  config.vm.define "vb" do |vb|

    vb.vm.network "private_network", ip: "192.168.57.10"

    vb.vm.provision "shell", inline: <<-SHELL
      #Actualizamos el sistema e instalamos Nginx
      apt-get update
      apt-get install -y nginx
      apt-get install -y vsftpd
      apt-get install -y git
      sudo systemctl start nginx

      #Eliminamos el directorio si ya existia para evitar problemas
      sudo rm -rf /var/www/webSamu/html/

      #Creamos la carpeta donde va a estar el sitio web
      sudo mkdir -p /var/www/webSamu/html
      sudo mkdir -p /var/www/webPedrosa/html

      #Clonamos el repositorio
      cd /var/www/webSamu/html
      sudo git clone https://github.com/cloudacademy/static-website-example

      #Cambiamos permisos y propietario del sitio web
      sudo chown -R www-data:www-data /var/www/webSamu/html/static-website-example
      sudo chmod -R 755 /var/www/webSamu/html

      sudo chown -R vagrant:vagrant /var/www/webPedrosa/html
      sudo chmod -R 755 /var/www/webPedrosa/html

      #Movemos el archivo de configuración para el sitio en sites-available
      cp /vagrant/webSamu /etc/nginx/sites-available/webSamu
      cp /vagrant/webPedrosa /etc/nginx/sites-available/webPedrosa

      #Creamos un enlace simbólico en sites-enabled
      sudo ln -sf /etc/nginx/sites-available/webSamu /etc/nginx/sites-enabled/
      sudo ln -sf /etc/nginx/sites-available/webPedrosa /etc/nginx/sites-enabled/

      #Verificamos la configuración de Nginx y reiniciamos el servicio
      sudo nginx -t
      sudo systemctl restart nginx

      cp /vagrant/vsftpd.conf /etc/vsftpd.conf

      sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
        -keyout /etc/ssl/private/vsftpd.key \
        -out /etc/ssl/certs/vsftpd.crt \
        -subj "/C=ES/CN=webSamu"  
      
      sudo systemctl restart vsftpd
    SHELL
  end
end
