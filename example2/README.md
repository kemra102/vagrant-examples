# Example 2

Lets start this example by introducing you to a new command:

```bash
$ vagrant status
```

You should see the status of any vagrant boxes your `Vagrantfile` defines. In this example you can see 2 of them! You can get the individual status of one of the boxes but the information given is no different to seeing the status for all the boxes.

Let's dive into our config a like last time. This time there a re a few more things going on though! Firstly lets take a look at the top few lines:

```ruby
nodes = [
  {
    hostname: 'server',
    ip: '192.168.199.90',
    ram: '1024'
  },
  {
    hostname: 'client',
    ip: '192.168.199.91'
  }
]
```

This is a pure Ruby Array of Hashes that contains the data for each server. Notice that both hashes don't contain the same value? Well that's fine as you'll see later on in our code. The next thing we see is different is on line 14:

```ruby
nodes.each do |node|
```

This bit of code for the beginner Rubyists in the audience takes each hash in our `nodes` array and performs certain actions over it. Lets take a look at those actions shall we? These actions constitue lines 15-29 of our `Vagrantfile` and are very similar to building our node in the previous example. So rather than re-iterating everything let's take a look at the differences.

```ruby
config.vm.define node[:hostname] do |nodeconfig|
```

This section is something that Vagrant needs when defining multiple nodes in one `Vagrantfile` in this way.

Lets look at something a little more interesting:

```ruby
nodeconfig.vm.box = node[:box] ? node[:box] : 'bento/centos-7.1'
```

Again established Rubyists can probably see what this does. For those paying attention you can easily see that this is the line that defines which box we want to use for our vagrant box. The difference is here we are getting the box name from our `nodes` ruby hash, and if we don't define the box there we default back to `bento/centos-7.1`.

Next we define our hostname (again Rubyists may see what's going on here):

```ruby
nodeconfig.vm.hostname = node[:hostname] + '.box'
```

Again we grab the `hostname` from our `nodes` ruby hash and concatonate the `.box` domain name onto it.

The rest of the lines you can easily follow at this point as there's nothing else new style wise. Even though it's multiple nodes you build them like you did previously:

```bash
$ vagrant up
```
