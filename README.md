# How to set up Djanjo with PostgreSQL on Ubuntu 16.04

## Setup Ubuntu 16.04
- Either follow [this guide](https://www.digitalocean.com/community/tutorials/initial-server-setup-with-ubuntu-16-04) to do it properly on a remove server OR
- Pick box from [Vagrant](https://app.vagrantup.com/boxes/)
- I chose [ubuntu/xenial64](https://app.vagrantup.com/ubuntu/boxes/xenial64)
  - Create & navigate to new folder in terminal (I'm calling mine `DjP` cloning it from an empty GitHub & copying in the .gitignore from my [DjangoPostgres](https://github.com/Aqueum/DjangoPostgres/blob/master/.gitignore))
  - `vagrant init ubuntu/xenial64`
  - edit vagrant file
    - change `# config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
`
    - to `config.vm.network "forwarded_port", guest: 8000, host: 8000, host_ip: "127.0.0.1"`
  - `vagrant up` if first time or `vagrant reload` to load the new vagrant file
  - `vagrant box update` to get latest version
  - `vagrant ssh` to SSH into your new box