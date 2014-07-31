## Install the SSH key

Install your public key on the remote server
```sh
cat ~/.ssh/id_rsa.pub | ssh root@12.12.12.12 "mkdir ~/.ssh; cat >> ~/.ssh/authorized_keys"
```
Enter the remote machine's password when prompted

Verify that you can login without a password

```sh
ssh root@12.12.12.12
```

## Add the server to the Ansible inventory file

Create an inventory file in the current directory

```ini
[vagrant]
vagrant ansible_ssh_port=2222 ansible_ssh_host=127.0.0.1

[another]
13.13.13.13

[production]
12.12.12.12
```

## Kick off Ansible

To provision the Vagrant box 

    ansible-playbook -i inventory webservers.yml --limit vagrant

Although, you shouldn't need to manually do it that way if you put this in your Vagrantfile

```ruby
config.vm.provision "ansible" do |ansible|
  ansible.playbook = "webservers.yml"
  ansible.verbose = 'vv'
  ansible.groups = { 'vagrant' => ['default']}
end
```
