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
      :ssh_port => '2200'
    },
    {
      :hostname => "vm2",
      :box      => "bento/ubuntu-24.04",
      :ip       => "172.16.10.51",
      :ssh_port => '2201'
    },
    {
      :hostname => "vm3",
      :box      => "bento/ubuntu-24.04",
      :ip       => "172.16.10.52",
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

vm1  running (virtualbox)
vm2  running (virtualbox)
vm3  running (virtualbox)
```

---

## SSH into a specific VM

Use the hostname to pick which one:

```bash
vagrant ssh vm1
vagrant ssh vm2
vagrant ssh vm3
```

---

## VM-to-VM Communication

All three VMs are connected to the same private network (`172.16.10.0/24`), which means they can communicate directly with each other **without** going through your host machine.

### Understanding the Network Setup

Each VM has:
- A **private IP address** (defined in the Vagrantfile)
- **SSH port forwarding** (so you can SSH from your host)

| VM | Private IP | SSH Port (from host) |
|----|-----------|----------------------|
| vm1 | 172.16.10.50 | 2200 |
| vm2 | 172.16.10.51 | 2201 |
| vm3 | 172.16.10.52 | 2202 |

### SSH Directly Between VMs

You can SSH from one VM to another using their private IP addresses:

**From your host, SSH into vm1:**
```bash
vagrant ssh vm1
```

**Inside vm1, SSH to vm2 using its private IP:**
```bash
ssh vagrant@172.16.10.51
```

**Or SSH to vm3:**
```bash
ssh vagrant@172.16.10.52
```

When prompted for a password, enter `vagrant` (the default Vagrant password).

### Example: Multi-VM Communication Chain

1. SSH into vm1 from your host:
   ```bash
   vagrant ssh vm1
   ```

2. From vm1, ping vm2 to verify connectivity:
   ```bash
   ping -c 3 172.16.10.51
   ```

3. SSH from vm1 to vm2:
   ```bash
   ssh vagrant@172.16.10.51
   ```

4. From vm2, you can reach vm1 or vm3:
   ```bash
   ssh vagrant@172.16.10.50    # to vm1
   ssh vagrant@172.16.10.52    # to vm3
   ```

5. Test connectivity:
   ```bash
   ping -c 3 172.16.10.50    # ping vm1 from vm2
   ping -c 3 172.16.10.52    # ping vm3 from vm2
   ```

### SSH Key-Based Authentication (Optional)

To avoid typing passwords, you can set up SSH key-based authentication. Inside any VM:

```bash
# Generate a key (if you don't have one)
ssh-keygen -t rsa -b 4096 -f ~/.ssh/id_rsa -N ""

# Copy it to another VM
ssh-copy-id -i ~/.ssh/id_rsa.pub vagrant@172.16.10.51
```

Now you can SSH without a password!

---

## Stop or destroy one VM

```bash
vagrant halt vm2       # stop just vm2
vagrant destroy vm3    # delete just vm3
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
