# Example 1

This is the most basic (more or less) VM you can create using Vagrant. As this is the first example lets break down the code:

```ruby
Vagrant.configure("2") do |config|
```

This line determines the [Configuration Version](https://www.vagrantup.com/docs/vagrantfile/version.html) of Vagrant that we want to use and create the config object that represents the vagrant box we will configure (the keen eyed may have already noticed this is actually just domain specific Ruby code).

```ruby
config.vm.box = 'bento/centos-7.1'
```

The name of the base box to start with - note the use of namespaces so different providors can provide boxes with the same name easily. As we haven't provided a URL in this config Vagrant assumes we want to fetch the base box from [Atlas](https://www.hashicorp.com/atlas.html) - Hashicorp's hosted solution for building & hosting Vagrant boxes.

```ruby
config.vm.hostname = 'centos.box'
```

The hostname assigned to our Vagrant box, simple.

```ruby
config.vm.network :private_network, ip: '192.168.199.90'
```

The basic network configuration for our Box, we create a private network with the specified IP assigned to our box. More [advanced configs](https://www.vagrantup.com/docs/networking/) will be used in subsequent examples.

```ruby
config.vm.provider :virtualbox do |vb|
  vb.customize [
    'modifyvm', :id,
    '--cpuexecutioncap', '50',
    '--memory', '512',
  ]
end
```

This one is a bit more complex and can actually be omitted if you want Vagrant to make these decisions for you. These values set the resoruces assigned to the Vagrant Box in regards to CPU shares and memory. For full details on these settings you should take a gander at [the official docs](https://www.vagrantup.com/docs/virtualbox/configuration.html).

Now you have some idea of what's what, lets do something a bit more interesting. To spin up the Vagrant box navigate into the `example1/` directory and run the following command:

```bash
$ vagrant up
```

This will download the box (if this is your first time), import it into Vagrant and spin up the Vagrant box according to our `Vagrantfile`.

Once it's up you can login if you like:

```bash
$ vagrant ssh
```

It's basically a standard CentOS 7 box with all the usual bells and whistles so feel free to explore. You have full password-less sudo access too!

When you're done you can clean up by destroying the Box:

```bash
$ vagrant destroy
```

You'll be asked if you're sure, just type in `y` and hit enter - and the Box is gone!
