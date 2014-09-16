Based on / inspired by:

* https://github.com/dodecaphonic/ansible-rails-app
* https://github.com/radar/ansible-rails-app
* https://github.com/7hunderbird/ansible-rails-app

It installs:

- nginx
- Puma (jungle)
- Ruby 2.1.2 (from source)
- PostgreSQL
- memcached
- Redis (for Sidekiq)

Change the app name, host and deploy directory in <code>vars/defaults.yml</code>.

To run:

    $ ansible-playbook ruby-webapp.yml

It also comes with a `Vagrantfile` for local testing and development.  Please see below on how to setup the local development environment.

## Vagrant setup

### Install Vagrant & Ansible

* <https://www.virtualbox.org/wiki/Downloads>
* <https://www.vagrantup.com/downloads>
* <http://docs.ansible.com/intro_installation.html#installing-the-control-machine>

`brew install ansible` works


### Copy hosts file

In this repositories' root folder, make a copy of the `hosts.example` file to a `host` file.

```
cp hosts.example hosts
```

Follow the next step to configure the ansible `hosts` file so you can use it with Vagrant.

### Insecure private key

Power on your Vagrant virtual server.

```
vagrant up
```

After the machine is up, you'll need to get the path to your `insecure_private_key` for the ansible `hosts` file.  Run the command:

```
vagrant ssh-config
```

And the output will look something like:

```
Host default
  HostName 127.0.0.1
  User vagrant
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /path/to/vagrant/insecure_private_key
  IdentitiesOnly yes
  LogLevel FATAL
```

Copy the `/path/to/vagrant/insecure_private_key` from the output and update the value of the `ansible_ssh_private_key_file` in your ansible `hosts` file.

### Setup `/etc/hosts` file

I have hardcoded the IP address in the `Vagrantfile` to `10.11.12.13`. Change as needed.

You'll need to setup your local `/etc/hosts` file to fully qualify and resolve the hostname you want to test.

For example, if you want to test `mywebapp.vagrant` on IP address `10.11.12.13`, then you need to add the following entry to your `/etc/hosts` file:

```
10.11.12.13    mywebapp.vagrant
```

### Ping test

Run the ping test to see if you've setup your configuration correctly:

```
ansible mywebapp.vagrant -i hosts -m ping
```

### Configurable variables

Open, in an editor, `vars/defaults.yml` for configurable variables.

## Run the playbook

    $ ansible-playbook -i hosts ruby-webapp.yml

or, to only run certain "tags":

    $ ansible-playbook -i hosts ruby-webapp.yml -t ruby,postgresql
