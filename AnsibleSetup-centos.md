$ sudo dnf install -y epel-release
$ sudo dnf update
$ sudo dnf install ansible -y
$ ansible --version
Create a new user called ansible and assign it to sudo group
$ adduser ansible
 $ passwd ansible
 $ usermod -aG wheel ansible
 
Login as ansible and allow sudo access without password for the login user in Remote Machine for Ansible to run any root commands
$ su ansible
$ echo "$(whoami) ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/$(whoami)

Preparing Debian 10 Remote Machine
$ sudo adduser ansible
$ sudo usermod -aG sudo ansible #add to sudo group
$ getent group sudo #check the member of sudo group
sudo:x:27:kwyong,ansible

$ su ansible
$ echo "$(whoami) ALL=(ALL) NOPASSWD:ALL" | sudo tee /etc/sudoers.d/$(whoami)

Prepare SSH Key
Stitch to ansible and generate a new SSH key to be deployed to remote machine

$ su ansible
$ ssh-keygen
$ ssh-copy-id ansible@ipaddr

# SSH Agent to avoid typing password for private key file
$ ssh-agent $SHELL
$ ssh-add #Enter your password for Private Key
$ ssh ansible@debian.aventis.dev # no password is required now


Create an Ansible Inventory File in /home/ansible
$ nano hosts

[debian]
debian.aventis.dev ansible_user=ansible
[centos]
192.168.1.114 ansible_user=ansible

List all hosts defined
$ ansible -i hosts --list-hosts all

Verify the hosts are active with PING
$ ansible -i hosts -m ping all


