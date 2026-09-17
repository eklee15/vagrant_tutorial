# Vagrant Tutorial

Vagrant lets you create and manage virtual machines from your terminal. Think of it like a recipe — you write what kind of computer you want, and Vagrant builds it for you!

---

## What You Need First: Install Vagrant

### Mac

```bash
brew install vagrant
```

> Don't have Homebrew? Install it first: https://brew.sh

### Windows

1. Download the installer from https://developer.hashicorp.com/vagrant/downloads
2. Run the `.msi` file and follow the prompts
3. Restart your terminal, then check it works:

```bash
vagrant --version
```

### Linux (Ubuntu/Debian)

```bash
wget -O - https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
sudo apt update && sudo apt install vagrant
```

You also need **VirtualBox** (the engine that runs the VMs):
- Download from https://www.virtualbox.org/wiki/Downloads and install it.

---

## What is a Vagrantfile?

A `Vagrantfile` is just a text file that tells Vagrant what kind of machine to build. You write it once, and anyone can use it to get the exact same machine.

Here is the simplest possible example:

```ruby
Vagrant.configure("2") do |config|
  config.vm.box      = "bento/ubuntu-24.04"  # which OS to use
  config.vm.hostname = "my-vm"               # the machine's name
end
```

---

## Tutorial Steps

| Step | What you'll learn |
|------|------------------|
| [Step1-firstvm](Step1-firstvm/README.md) | Create your very first VM |
| [Step2-options](Step2-options/README.md) | Shared folders, file copying, memory & CPU |
| [Step3-commands](Step3-commands/README.md) | The most useful Vagrant commands |
| [Step4-multihost](Step4-multihost/README.md) | Run multiple VMs at once |

Start with Step 1 and work your way down!
