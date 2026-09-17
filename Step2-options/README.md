# Step 2: More Options

Now let's make our VM more useful — share files, copy files in, and control memory & CPU.

---

## The Vagrantfile for this step

```ruby
Vagrant.configure("2") do |config|
  config.vm.box      = "bento/ubuntu-24.04"
  config.vm.hostname = "options-vm"
  config.vm.network "private_network", ip: "172.16.10.11"

  # Share a folder between your computer and the VM
  config.vm.synced_folder "./data", "/home/vagrant/data"

  # Copy a file into the VM at startup
  config.vm.provision "file", source: "./welcome.txt", destination: "/home/vagrant/welcome.txt"

  # Set memory and CPU
  config.vm.provider "virtualbox" do |vb|
    vb.memory = 2048   # 2 GB RAM
    vb.cpus   = 2      # 2 CPU cores
  end
end
```

---

## Shared Folder: `vm.synced_folder`

```ruby
config.vm.synced_folder "./data", "/home/vagrant/data"
```

- `"./data"` — a folder on **your computer** (next to the Vagrantfile)
- `"/home/vagrant/data"` — where it appears **inside the VM**

Files you put in `./data` on your machine show up instantly inside the VM — and vice versa!

### Try it

1. Create the folder and a test file:

```bash
mkdir data
echo "Hello from my computer!" > data/test.txt
```

2. Start the VM and check:

```bash
vagrant up
vagrant ssh
cat /home/vagrant/data/test.txt   # prints: Hello from my computer!
```

---

## Copy a File: `vm.provision "file"`

```ruby
config.vm.provision "file", source: "./welcome.txt", destination: "/home/vagrant/welcome.txt"
```

This copies `welcome.txt` from your machine into the VM **once**, when the VM is first created.

### Try it

1. Create the file:

```bash
echo "Welcome to your VM!" > welcome.txt
```

2. Start the VM and check:

```bash
vagrant up
vagrant ssh
cat /home/vagrant/welcome.txt   # prints: Welcome to your VM!
```

---

## Memory and CPU

```ruby
config.vm.provider "virtualbox" do |vb|
  vb.memory = 2048   # megabytes — 2048 = 2 GB
  vb.cpus   = 2
end
```

Change the numbers to give your VM more or less power. After changing, run:

```bash
vagrant reload
```

to apply the new settings.

---

Next: [Step 3 — Useful Vagrant commands](../Step3-commands/README.md)
