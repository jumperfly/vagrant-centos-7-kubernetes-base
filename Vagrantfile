# -*- mode: ruby -*-
# vi: set ft=ruby :

$parent_box = "jumperfly/centos-7-ansible"
$parent_box_version = "1809.01.01"

Vagrant.configure("2") do |config|
  config.vm.box = $parent_box
  config.vm.box_version = $parent_box_version
  config.vm.box_check_update = false
  config.ssh.insert_key = false

  config.vm.provider "virtualbox" do |v|
    v.memory = 256
    v.cpus = 1
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "site.yml"
    ansible.galaxy_role_file = "requirements.yml"
  end

  config.vm.provision "shell", inline: <<-SHELL
    # Cleanup
    rm -f /etc/ssh/*key*
    yum clean all
    rm -rf /var/cache/yum
    dd if=/dev/zero of=/ZERO bs=1M
    rm /ZERO
    swapoff /dev/mapper/VolGroup00-LogVol01
    dd if=/dev/zero of=/dev/mapper/VolGroup00-LogVol01 bs=1M
    mkswap /dev/mapper/VolGroup00-LogVol01
    sync
    echo "All done! Machine will now shutdown ready for a 'vagrant package'..."
    history -c && poweroff
  SHELL
end
