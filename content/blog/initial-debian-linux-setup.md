+++
title = "Initial Debian-based Linux Setup for a Minimalist Programmer"
date = 2026-02-28
authors = ["Hildeberto Mendonca",]
description = "This is all a programmer needs to build software in a minimalist way."
+++

This setup is intended for programmers who want to solve problems with simple
solutions.

### sudo Command

Ubuntu and its derivatives already add the initial user in the SUDO group, but
Debian requires us to explicitly add the user to the sudo group. This is how
it is done: 

```bash
$ su -
  # usermod -aG sudo username
  # exit
```

Logout and login to the changes take effect.

### Install Dropbox

Dropbox is the most complete file sharing service available for Linux. Since
we don't have a high volume of files to store, it is still the only solution
that saves us time by not having to deal with a physical storage.

First we install the dependencies:

```bash
$ sudo apt install python3-gpg
$ sudo apt install libappindicator3-1
```

then we follow the instructions on [Dropbox's website](https://www.dropbox.com/install-linux).

It might be necessary to increase the system's inotify file watch limit:

```bash
$ cat /proc/sys/fs/inotify/max_user_watches
$ sudo vi /etc/sysctl.conf 
    fs.inotify.max_user_watches=1000000
$ sudo sysctl --system
```

Then stop and restart Dropbox:

```bash
$ dropbox stop
$ dropbox start -i
```

[Proton Drive](https://www.proton.me/drive) is an alternative to Dropbox but
with several limitations on Linux. It offers a terminal client called 
[Proton Drive CLI](https://www.proton.me/blog/proton-drive-cli), developed on
top of [Proton Drive SDK](https://www.proton.me/blog/drive-sdk-june-2026), as
an initial step on Linux, but they still have a lot of work to do.

### Install Git

A more comprehensive Git installation is covered in our
[**Git Experiences**](/library/publications/) publication, available in the
[Library](/library/) section. It is required for the upcoming steps.

### Install zsh and Oh-My-Zsh

```bash
$ sudo apt install zsh
```

To install **Oh-My-Zsh**, follow the steps on the article
[Installing Oh-My-Zsh on Debian-Based Systems](/blog/oh-my-zsh/).

### Install VSCode and Zed

[VSCode](https://code.visualstudio.com) still has more features and support for
more technologies. That's why it is still relevant. So, here is how we install it:

1. Download the installation file from the website and follow the installation
   instructions

2. Configure the operating system to allow watching more files for changes

    ```bash
    $ cat /proc/sys/fs/inotify/max_user_watches
    $ sudo vi /etc/sysctl.conf 
        fs.inotify.max_user_watches=1000000
    $ sudo sysctl --system
    ```

3. Go to **File > Preferences > Settings** and search for "Line Height", then
   set it to 26, which is a comfortable spacing between lines.

[Zed](https://zed.dev/) is slowly becoming my favorite IDE, specially because
of its performance and usage of the GPU. The installation is easier:

```bash
curl -f https://zed.dev/install.sh | sh
```

### Install Rust

First, install the basic requirements for `rustup`:

```bash
$ sudo apt install build-essential
$ sudo apt install curl
```

Now, intall `rustup`, the tool that manages Rust installation:

```bash
$ curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

It prints the following message, describing the installation. I recommend
taking note of it for future reference:

```
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

```bash
$ rustup update
$ rustup toolchain install stable
```

To have a better development experience, install Cargo Watch:

```bash
$ cargo install cargo-watch
$ cargo watch -x check -x run
```

It recompiles the project everytime the source code changes.

### Install Go

Download the Go installation file from [its website](https://go.dev/dl/) and
continue in the console:

```bash
$ sudo rm -rf /usr/local/go
$ sudo tar -C /usr/local -xzf go1.26.2.linux-amd64.tar.gz
$ cd $ZSH_CUSTOM
$ vi env.zsh
  export PATH=$PATH:/usr/local/go/bin
  export PATH=$PATH:~/go/bin

[reopen]
$ go version
```

### Install PostgreSQL

```bash
$ sudo apt install postgresql
$ sudo su - postgres -c "createuser -s $USER"
$ createdb
```

Refer to the publication [PostgreSQL Experences](/postgres-experiences/database.html)
to create application databases when needed.

### Install Claude Code

```bash
$ curl -fsSL https://claude.ai/install.sh | bash
$ echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.zshrc && source ~/.zshrc
```
