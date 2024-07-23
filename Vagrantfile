# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  config.vm.define "ubuntu" do |vb|
    vb.vm.box = "ubuntu/jammy64"
    vb.vm.host_name = "otuslinux"
    vb.vm.network "private_network", ip: "192.168.56.101"
    vb.vm.provider :virtualbox
    (1..5).each do |i|
      vb.vm.disk :disk, size: "250MB", name: "disk-#{i}"
      vb.vm.provision "shell", inline: <<-SHELL
      mkdir -p ~root/.ssh
            cp ~vagrant/.ssh/auth* ~root/.ssh
      apt install -y mdadm smartmontools hdparm gdisk
      mdadm --zero-superblock --force /dev/sd{c,d,e,f,g}
      mdadm --create --verbose /dev/md0 -l 5 -n 5 /dev/sd{c,d,e,f,g}
      echo "DEVICE partitions" > /etc/mdadm/mdadm.conf
      mdadm --detail --scan --verbose | awk '/ARRAY/ {print}' >> /etc/mdadm/mdadm.conf
      mkfs.ext4 /dev/md0
      mkdir /mnt/01
      mount /dev/md0 /mnt/01
    SHELL
    end
  end
end