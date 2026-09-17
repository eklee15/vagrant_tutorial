# Step 4: Multiple VMs

Sometimes you need more than one machine — for example, a web server and a database server. Vagrant can create all of them from a single `Vagrantfile`!

---

## The Vagrantfile

Instead of one machine, we define a list and loop through it:

```ruby
Vagrant.configure("2") do |config|
  servers = [
    {
      :hostname => "vm1",
      :box      => "bento/ubuntu-24.04",
      :ip       => "172.16.10.50",
      :ssh_port => '22'
    },
    {
      :hostname => "Server2",
      :box      => "bento/ubuntu-24.04",
      :ip       => "172.16.10.51",
      :ssh_port => '2201'
    },
    {
      :hostname => "Server3",
      :box      => "bento/ubuntu-24.04",
      :ip       => "192.168.56.102",
      :ssh_port => '2202'
    }
  ]

  servers.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box      = machine[:box]
      node.vm.hostname = machine[:hostname]
      node.vm.network :private_network, ip: machine[:ip]
      node.vm.network "forwarded_port", guest: 22, host: machine[:ssh_port], id: "ssh"
      node.vm.synced_folder "../data", "/home/vagrant/data"
      node.vm.provision "file", source: "./copiedfile.txt", destination: "/home/vagrant/copiedfile.txt"

      node.vm.provider :virtualbox do |vb|
        vb.customize ["modifyvm", :id, "--memory", 2048]
        vb.customize ["modifyvm", :id, "--cpus", 2]
      end
    end
  end
end
```

---

## How it works

The `servers` list has one entry per VM. Each entry has:

| Key | What it is |
|-----|-----------|
| `:hostname` | The VM's name |
| `:box` | Which OS image to use |
| `:ip` | Private network IP address |
| `:ssh_port` | Port on your computer to reach this VM via SSH |

The `servers.each` loop creates one VM for every entry in the list. To add more VMs, just add more entries to the list!

---

## Start all VMs

```bash
vagrant up
```

All three machines start at once.

---

## Check status

```bash
vagrant status
```

Example output:
```
current machine states:

vm1      running (virtualbox)
Server2  running (virtualbox)
Server3  running (virtualbox)
```

---

## SSH into a specific VM

Use the hostname to pick which one:

```bash
vagrant ssh vm1
vagrant ssh Server2
vagrant ssh Server3
```

---

## Stop or destroy one VM

```bash
vagrant halt Server2       # stop just Server2
vagrant destroy Server3    # delete just Server3
```

Or stop/destroy all at once:

```bash
vagrant halt
vagrant destroy
```

---

## Add your own VM

To add a 4th machine, add one more entry to the `servers` list in the Vagrantfile:

```ruby
{
  :hostname => "Server4",
  :box      => "bento/ubuntu-24.04",
  :ip       => "172.16.10.53",
  :ssh_port => '2203'
},
```

Then run `vagrant up Server4` to start just the new one.

---

You've finished the tutorial! You can now create, configure, and manage virtual machines with Vagrant.
