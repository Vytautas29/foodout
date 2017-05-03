# -*- mode: ruby -*-
# vi: set ft=ruby :

$phpmyadmin_install = <<SCRIPT
    APP_PASS="root"
    ROOT_PASS="root"
    APP_DB_PASS="root"
    echo "phpmyadmin phpmyadmin/dbconfig-install boolean true" | debconf-set-selections
    echo "phpmyadmin phpmyadmin/app-password-confirm password $APP_PASS" | debconf-set-selections
    echo "phpmyadmin phpmyadmin/mysql/admin-pass password $ROOT_PASS" | debconf-set-selections
    echo "phpmyadmin phpmyadmin/mysql/app-pass password $APP_DB_PASS" | debconf-set-selections
    echo "phpmyadmin phpmyadmin/reconfigure-webserver multiselect apache2" | debconf-set-selections
    apt-get install -y phpmyadmin
    sed -i '/open_basedir/d' /etc/phpmyadmin/apache.conf
    service apache2 restart
SCRIPT

Vagrant.configure("2") do |config|

    config.vm.box = "scotch/box"
    config.vm.network "private_network", ip: "192.168.33.10"
    config.vm.hostname = "scotchbox"
    #config.vm.synced_folder ".", "/var/www", :mount_options => ["dmode=777", "fmode=666"]

    # Optional NFS. Make sure to remove other synced_folder line too
    config.vm.synced_folder ".", "/var/www", id: "v-root", mount_options: ["rw", "tcp", "nolock", "noacl", "async"], type: "nfs", nfs_udp: false

    config.vm.provision "shell", inline: $phpmyadmin_install

end
