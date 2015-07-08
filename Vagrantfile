# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  config.vm.box = "deimosfr/debian-jessie"
  config.ssh.insert_key = false
  config.ssh.forward_agent = true

  config.vm.provision :ansible do |ansible|
    ansible.sudo = true
    ansible.host_key_checking = false
    ansible.raw_ssh_args = ['-o UserKnownHostsFile=/dev/null']
    ansible.playbook = "test.yml"
    ansible.extra_vars = {
      ansible_ssh_user: 'vagrant',
      ansible_connection: 'ssh',
      ansible_ssh_args: '-o ForwardAgent=yes -A',
    }
  end
end
