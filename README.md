Get an OpenStax Tutor and Canvas development environment up and running.

## USAGE

Start with:

    $ vagrant up

This will spin up the VM, install all dependencies, and do all necessary prerequisites.

To connect to the running VM, run:

    $ vagrant ssh

To shut down and completely clean up the VM, run:

    $ vagrant destroy

For all other management of the VM, refer to Vagrant's documentation.

This VM has Canvas pre-installed, but not running.  And then it is up to you the developer to clone accounts, tutor-server, and tutor-js inside this repository.  They are `.gitignore`d by this repo, and you'll be able to work in them independently with git.  By cloning them inside this repo, these projects will be mounted inside the VM in the `/vagrant` directory.  So you'll be able to run code from within the VM (and all the sites will be able to see each other on `localhost`), but use your favorite editor to edit them from the host computer as well as run git commands from the host.

For each site, you should open a separate terminal on your machine, *starting in this directory*, and do the following.  With separate terminals, you'll be able to monitor the logs of each to make sure things are talking to each other.

### Canvas

```
$host> vagrant ssh
$guest> cd /apps/canvas
$guest> bundle exec rails s -b 0.0.0.0
```

### accounts

```
$host> git clone git@github.com:openstax/accounts.git
$host> vagrant ssh
$guest> cd /vagrant/accounts
$guest> ruby --version # should show 2.3.3
$guest> gem install bundler
$guest> bundle install --without production
$guest> bundle exec rake db:migrate
$guest> bundle exec rails s -b 0.0.0.0
```

### tutor-server

```
$host> git clone git@github.com:openstax/tutor-server.git
$host> vagrant ssh
$guest> cd /vagrant/tutor-server
$guest> ruby --version # should show 2.2.3
$guest> gem install bundler
$guest> bundle install --without production
$guest> bundle exec rake db:migrate db:seed
$guest> # whatever demo command you want
$guest> bundle exec rails s -b 0.0.0.0
```

Of course, you don't run all these steps everytime.

All other OpenStax-y development is the same.  You'll have to set up oauth keys between Tutor and Accounts, point Tutor to some Exercises instance, etc.

### Notes

1. Without `-b 0.0.0.0`, the servers won't be visible outside of the VM.
2. There is no `monit` running, so if postgres or redis dies, you'll have to handle it.

## PREREQUISITES

You must [VirtualBox](https://www.virtualbox.org/) and [Vagrant](https://www.vagrantup.com) installed on your workstation.  It is possible that you just need Vagrant or that Vagrant will talk to you about installing VirtualBox.

## Shout Out

Thanks to David Adams for his [canvas-vagrant repo](https://github.com/daveadams/canvas-vagrant) on which this repo was based.
