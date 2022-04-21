---
title: "Toolbox Vscode Setup Connect ssh"
created: 2022-01-19 11:56:15
tags: container
keywords: containers, toolbox, vscode, code, code-insiders, ssh
---

# Toolbox Vscode Setup Connect ssh

links
: [Integrating Fedora Toolbox into VS Code (with the help of SSH)](https://jurf.github.io/2020/03/24/vscode-toolbox/)

## Toolbox Visual Studio Code integration

[VS Code](https://flathub.org/apps/details/com.visualstudio.code) has become my favourite editor as of late, however, using it out of a Flatpak environment on Fedora Silverblue is limited. You can [configure](https://discussion.fedoraproject.org/t/developing-applications-using-flatpak-packaged-editors-ides/269/19[]) the built-in terminal to run in Toolbox, but that doesn’t help if extensions need tooling installed.

Copying the approach of [CLion and WSL](https://github.com/JetBrains/clion-wsl), this is a straight-forward how-to for container-based development in VS Code.

First, [create and enter](https://docs.fedoraproject.org/en-US/fedora-silverblue/toolbox/#toolbox-first-toolbox) a toolbox. Once inside, we need to install and configure the SSH server.

```bash
⬢[user@toolbox ~]$ sudo dnf install openssh-server
```

On a regular Fedora system, launching sshd with systemctl would trigger sshd-keygen.target. We can’t do this in Toolbox, so we have to do it manually.

```bash
⬢[user@toolbox ~]$ sudo /usr/libexec/openssh/sshd-keygen rsa
⬢[user@toolbox ~]$ sudo /usr/libexec/openssh/sshd-keygen ecdsa
⬢[user@toolbox ~]$ sudo /usr/libexec/openssh/sshd-keygen ed25519
```

Inside /etc/ssh/sshd_config, ensure these options are set (assuming you are running Fedora 34 inside):

```bash
# For VS Code
Port 2234                 # Prevent conflicts with other SSH servers
ListenAddress localhost   # Don’t allow remote connections
PermitEmptyPasswords yes  # Containers lack passwords by default
PermitUserEnvironment yes # Allow setting DISPLAY for remote connections
```

I like including the Fedora version in the container in the port so that I can have multiple versions at the same time and don’t have to remove lines in ~/.ssh/known_hosts on container upgrades.

NB: Due to container limitations, interactive sessions [won’t work](https://discussion.fedoraproject.org/t/ssh-into-a-toolbox/2155/12).

Exit the toolbox. You can now run the server with a toolbox run sudo /usr/sbin/sshd. You can add this into your .bash_profile if you want it to run automatically.

Add this to your `~/.ssh/config`:

```text
Host toolbox-32
    HostName localhost
    Port 2234
```

And this to ~/.ssh/environment (this allows to starts Xorg apps from VS Code):

```bash
DISPLAY=:0
```

As I became tired of running all these commands on every container installation or upgrade, I created [this script](https://gist.github.com/jurf/6f04ce4f9bfa2268bce154fb6ea0a69b) to automate the process.

```bash
#!/bin/sh

sudo dnf install --assumeyes openssh-server
sudo /usr/libexec/openssh/sshd-keygen rsa
sudo /usr/libexec/openssh/sshd-keygen ecdsa
sudo /usr/libexec/openssh/sshd-keygen ed25519

echo "
# For VS Code
Port 22$VERSION                 # Prevent conflicts with other SSH servers
ListenAddress localhost   # Don’t allow remote connections
PermitEmptyPasswords yes  # Containers lack passwords by default
PermitUserEnvironment yes # Allow setting DISPLAY for remote connections" | sudo tee -a /etc/ssh/sshd_config

echo "
Host toolbox-$VERSION
    HostName localhost
    Port 22$VERSION" >> ~/.ssh/config

sudo /usr/sbin/sshd
```

view raw
toolbox-ssh-setup.sh hosted with ❤ by GitHub

Inside VS Code, install the ‘[Remote – SSH](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-ssh)’ extension, initialise a connection and pick ‘toolbox-34’ as the host, and enjoy.

[Bonus tip](https://discussion.fedoraproject.org/t/tip-use-var-home-instead-of-home-in-vs-code/21887): Open your projects via /var/home/… instead of /home/… to avoid symlink bugs.
