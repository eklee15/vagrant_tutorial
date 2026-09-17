# Step 3: Useful Vagrant Commands

Here are the commands you'll use every day. Run them all from inside the folder that has your `Vagrantfile`.

---

## `vagrant up`

**Starts your VM.** If it doesn't exist yet, Vagrant creates it first.

```bash
vagrant up
```

---

## `vagrant status`

**Shows whether your VM is running or stopped** — for the VMs in the current folder.

```bash
vagrant status
```

Example output:
```
current machine states:

first-vm                  running (virtualbox)
```

---

## `vagrant global-status`

**Shows ALL your VMs** across every folder on your computer.

```bash
vagrant global-status
```

Example output:
```
id       name      provider   state   directory
---------------------------------------------------------------
a1b2c3   first-vm  virtualbox running /home/you/Step1-firstvm
d4e5f6   options-vm virtualbox poweroff /home/you/Step2-options
```

---

## `vagrant ssh-config`

**Shows the SSH connection details** for your VM — useful if you want to connect with another tool.

```bash
vagrant ssh-config
```

Example output:
```
Host first-vm
  HostName 127.0.0.1
  User vagrant
  Port 2222
  IdentityFile /path/to/.vagrant/machines/first-vm/virtualbox/private_key
```

---

## `vagrant reload`

**Restarts your VM and re-reads the Vagrantfile.** Use this after you change settings like memory or CPU.

```bash
vagrant reload
```

---

## `vagrant halt`

**Stops (shuts down) your VM.** The VM and all its files are kept — it's just turned off.

```bash
vagrant halt
```

Start it again later with `vagrant up`.

---

## `vagrant destroy`

**Completely deletes the VM.** All data inside the VM is gone (your synced folders on your computer are safe).

```bash
vagrant destroy
```

Vagrant will ask you to confirm:
```
    default: Are you sure you want to destroy the 'first-vm' VM? [y/N] y
```

Type `y` and press Enter.

---

## Quick Reference

| Command | What it does |
|---------|-------------|
| `vagrant up` | Start / create the VM |
| `vagrant status` | Check if this VM is running |
| `vagrant global-status` | Check all VMs on your computer |
| `vagrant ssh-config` | Show SSH connection details |
| `vagrant reload` | Restart VM and apply Vagrantfile changes |
| `vagrant halt` | Shut down the VM (keep it) |
| `vagrant destroy` | Delete the VM completely |

---

Next: [Step 4 — Multiple VMs](../Step4-multihost/README.md)
