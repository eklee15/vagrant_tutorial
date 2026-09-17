# Step 1: Your First VM

Let's create a simple Linux virtual machine!

---

## 1. Create a folder and the Vagrantfile

The `Vagrantfile` is already here in this folder. It tells Vagrant to create one Ubuntu machine.

```ruby
Vagrant.configure("2") do |config|
  config.vm.box      = "bento/ubuntu-24.04"
  config.vm.hostname = "first-vm"
  config.vm.network "private_network", ip: "172.16.10.10"
end
```

What each line means:
- `vm.box` — which OS image to download (Ubuntu 24.04)
- `vm.hostname` — the name of your machine
- `vm.network "private_network"` — gives the VM its own private IP so you can reach it

---

## 2. Start the VM

Inside this folder, run:

```bash
vagrant up
```

The first time takes a few minutes — it downloads the Ubuntu image. After that it starts fast!

---

## 3. Log in to the VM

```bash
vagrant ssh
```

You are now inside the virtual machine! Try a few commands:

```bash
hostname        # should print: first-vm
ip a            # should show 172.16.10.10
uname -a        # shows the Linux version
```

---

## 4. Leave the VM

Type `exit` or press `Ctrl+D` to return to your normal terminal.

---

## 5. Stop the VM when you're done

```bash
vagrant halt
```

---

Next: [Step 2 — Shared folders, file copying, memory & CPU](../Step2-options/README.md)
