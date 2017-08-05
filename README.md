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


## PREREQUISITES

You must [VirtualBox](https://www.virtualbox.org/) and [Vagrant](https://www.vagrantup.com) installed on your workstation.  It is possible that you just need Vagrant or that Vagrant will talk to you about installing VirtualBox.

## Shout Out

Thanks to David Adams for his [canvas-vagrant repo](https://github.com/daveadams/canvas-vagrant) on which this repo was based.
