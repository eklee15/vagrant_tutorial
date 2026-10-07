# Step 5: Run a Docker Web Server Inside Your VM

In this step you will:
1. Start the VM from Step 1
2. Install Docker inside the VM
3. Clone a simple web server project
4. Build and run it as a Docker container
5. Open it in your browser from your Mac

---

## Prerequisites

- You completed [Step 1](../Step1-firstvm/README.md) and have the VM working
- You have the `Step1-firstvm` folder on your machine

---

## 1. Start the VM

Open your terminal and go to the Step 1 folder:

```bash
cd ../Step1-firstvm
vagrant up
vagrant ssh
```

You are now inside the VM. All commands from here on are run **inside the VM** unless noted.

---

## 2. Install Docker

Run these commands one at a time. Each step is explained below.

**Update the package list:**
```bash
sudo apt-get update
```

**Install required packages:**
```bash
sudo apt-get install -y ca-certificates curl gnupg
```

**Add Docker's official GPG key:**
```bash
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg
```

**Add the Docker repository:**
```bash
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```

**Install Docker:**
```bash
sudo apt-get update
sudo apt-get install -y docker-ce docker-ce-cli containerd.io
```

**Allow your user to run Docker without `sudo`:**
```bash
sudo usermod -aG docker $USER
newgrp docker
```

**Verify Docker is working:**
```bash
docker --version
```

You should see something like: `Docker version 27.x.x, build ...`

---

## 3. Install Git and Clone the Web Server Repo

**Install Git:**
```bash
sudo apt-get install -y git
```

**Clone the repo:**
```bash
git clone https://github.com/eklee15/my-docker-webserver.git
```

**Go into the project folder:**
```bash
cd my-docker-webserver
```

**Check that the files are there:**
```bash
ls
```

You should see a `Dockerfile` and an `html` folder.

---

## 4. Build the Docker Image

```bash
docker build -t my-webserv-test .
```

This reads the `Dockerfile` and builds an image named `my-webserv-test`. It may take a minute the first time.

**Confirm the image was created:**
```bash
docker images
```

You should see `my-webserv-test` in the list.

---

## 5. Run the Container

```bash
docker run -d -p 80:80 --name webserv-container my-webserv-test
```

What the flags mean:
- `-d` — run in the background (detached)
- `-p 80:80` — forward port 80 on the VM to port 80 in the container
- `--name webserv-container` — give the container a friendly name

**Check that it is running:**
```bash
docker ps
```

You should see `webserv-container` with status `Up`.

---

## 6. Open in Your Browser

The VM's private IP is `172.16.10.10` (set in the Step 1 Vagrantfile).

Open your browser on your Mac and go to:

```
http://172.16.10.10
```

You should see the web server's homepage.

> **Not loading?** Make sure the VM is still running (`vagrant status` from the Step1-firstvm folder) and the container is up (`docker ps` inside the VM).

---

## 7. Stop and Clean Up (Optional)

When you are done, you can stop and remove the container:

```bash
docker stop webserv-container
docker rm webserv-container
```

Exit the VM and shut it down:

```bash
exit
```

Back on your Mac:

```bash
vagrant halt
```

---

## Quick Reference

| Task | Command |
|---|---|
| Start the VM | `vagrant up` (from Step1-firstvm) |
| SSH into the VM | `vagrant ssh` |
| Check Docker version | `docker --version` |
| List running containers | `docker ps` |
| Stop a container | `docker stop webserv-container` |
| Remove a container | `docker rm webserv-container` |
| List images | `docker images` |
| Remove an image | `docker rmi my-webserv-test` |
| Halt the VM | `vagrant halt` |

---

Previous: [Step 1 — Your First VM](../Step1-firstvm/README.md)
