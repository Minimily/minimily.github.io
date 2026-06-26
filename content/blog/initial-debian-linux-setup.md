+++
title = "Initial Debian-based Linux Setup for a Minimalist Programmer"
date = 2026-02-28
authors = ["Hildeberto Mendonca",]
description = "This is all a programmer needs to build software in a minimalist way."
+++

## sudo Command

Ubuntu and its derivatives already add the initial user in the SUDO group, but
Debian requires us to explicitly add the user to the sudo group. This is how it is done: 

```
$ su -
# usermod -aG sudo username
# exit
```

Logout and login to the changes take effect.

### Install Dropbox

Dependency:

```
$ sudo apt install python3-gpg
$ sudo apt install libappindicator3-1
```

### Install Git

```
$ sudo apt install git
```

### Install zsh and oh-my-zsh

```
$ sudo apt install zsh
$ sh -c "$(wget https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh -O -)"
```

### Install VSCode and Zed

```
$ cat /proc/sys/fs/inotify/max_user_watches
$ sudo vi /etc/sysctl.conf 
      fs.inotify.max_user_watches=524288
$ sudo sysctl --system
```

### Install Rust

First, install the basic requirements for `rustup`:

```
$ sudo apt install build-essential
$ sudo apt install curl
```

Now, intall `rustup`, the tool that manages Rust installation:

```
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh

  This will download and install the official compiler for the Rust
  programming language, and its package manager, Cargo.

  Rustup metadata and toolchains will be installed into the Rustup
  home directory, located at:

    /home/[username]/.rustup

  This can be modified with the RUSTUP_HOME environment variable.

  The Cargo home directory is located at:

    /home/[username]/.cargo

  This can be modified with the CARGO_HOME environment variable.

  The cargo, rustc, rustup and other commands will be added to
  Cargo's bin directory, located at:

    /home/[username]/.cargo/bin

  This path will then be added to your PATH environment variable by
  modifying the profile files located at:

    /home/[username]/.profile
    /home/[username]/.bashrc
    /home/[username]/.zshenv

  You can uninstall at any time with rustup self uninstall and
  these changes will be reverted.

[reopen]

$ cargo --version
```

From time to time, update your Rust installation:

```
$ rustup update
$ rustup toolchain install stable
```

### Install Go

```
$ sudo rm -rf /usr/local/go
$ sudo tar -C /usr/local -xzf go1.26.2.linux-amd64.tar.gz
[reopen]
$ go version
```

### Install PostgreSQL

```
$ sudo apt install postgresql
```

### Install Claude Code

```
$ curl -fsSL https://claude.ai/install.sh | bash
$ echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
```
