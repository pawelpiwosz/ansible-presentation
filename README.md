# Ansible playground

This excercise will show you how to use Ansible to install fully functional Wordpress.

## Prerequisities

In order to successfully run the Ansible excercise, you have to install:

* [VirtualBox](https://www.virtualbox.org/)
* [Vagrant](https://www.vagrantup.com/)

## Vagrant

After installation, you need to create two Vagrant machines, using this [Vagrantfile](./Vagrantfile)

To provision the machines, execute `vagrant up` in the directory where Vagrant file is. To check the status, use `vagrant status`. To connect to one of the machines, use `vagrant ssh <machine_name>`. And finally, to destroy both boxes, run `vagrant destroy -f`.

### Connect two Vagrant boxes using ssh key

Execute this sequence on `ansiblehost`

* `ssh-keygen -t ed25519`
* `cat .ssh/id_ed25519.pub` (or the name generated)

Copy the key.

From the host machine login to ansiblenode `vagrant ssh ansiblenode`, and execute

`echo "<insert your public key here" >> .ssh/authorized_keys`

Come back to ansiblehost. Add ssh key to ssh-agent.

* `eval $(ssh-agent)`
* `ssh-add .ssh/<keyname>`

Now you should be able to login to `ansiblenode`. IP of the box is in the Vagrantfile.

`ssh vagrant@10.100.198.189`

You successfuly established connection from host to node.

## Prepare ansible environment on `ansiblenode`

Install required packages by executing `sudo apt update && sudo apt install -y python3.8 python3-pip virtualenv`

Update version of Python in the system `sudo update-alternatives --install /usr/bin/python python /usr/bin/python3.8 1`

In `/datanode` directory you have two files. Copy both of them to you home directory.

Execute

* `chmod 755 envprep.sh`
* `./envprep.sh`

## Execute the virtualenv

run `. ansible/bin/activate`. Prompt should indicate that you have active virtualenv (for example `(ansible) vagrant@ansiblehost:`)

## Test Ansible

Execute `ansible --version` to confirm installation.

Now it is time to test if ansible is able to connect to `ansiblenode`. Simply execute this ad-hoc command:

`ansible vagrant_boxes -i inventory -a "hostname"`

You should receive:

```bash
10.100.198.189 | CHANGED | rc=0 >>
ansiblenode
```

If this is correct, you are ready to run playbooks.

### Interesting trick

If you wish to not run virtualenv and ansible configurations, you can put it into provisioning scripts.

## Run playbook

Execute the playbook against `inventory` file. To what hosts - it is defined in first lines of `provision.yml`.

`ansible-playbook -i inventory provision.yml`

## Credits

Ansible playbook is copied from [here](https://www.makarenalabs.com/ansible-for-it-automation-wordpress-as-an-example/). If time allows, I'll update it to newest version. Someday ;)
