# Linux

## Codium
1. Use this [link](https://gitlab.com/paulcarroty/vscodium-deb-rpm-repo)

## ExploitDB / Searchsploit
On \*nix systems, all you really need is either “CoreUtils” or “utilities” (e.g. bash, sed, grep, awk, etc.), as well as git. These are installed by default on many different Linux distributions, including OS X/macOS.

In `~/git`:
`git clone https://github.com/offensive-security/exploitdb.git`

To run:
`~/git/searchsploit [search term]`

## Fedora
1. (RHEL-based) `sudo dnf upgrade`
2. Install python3 dev tools so pip works: `sudo dnf install python3-devel`

### VMs
1. Look at instructions [here]([200~https://docs.fedoraproject.org/en-US/quick-docs/getting-started-with-virtualization/)
2. (not sure) Install `spice-vdagent` on guest OS?
3. `systemctl start spice-vdagent` on host (Fedora)
	1. Enable it?

### Espanso
1. Follow Wayland instructions and [compile from source](https://espanso.org/docs/install/linux/#install-on-wayland)
	1. copy to usual git folder in home directory (`~/git`)
	2. mv compiled binary to /usr/local/bin per instructions; consider a link in the future?
	3. `cp -r ~/git/dotfiles/espanso ~/.config`

### Vim
1. `sudo dnf install vim-enhanced` as it comes with vi-minimal initially
2. `cp ~/git/dotfiles/vimrc ~/.vimrc`

### VirtualBox
1. Disable Secure Boot before installing rpm downloaded from official site, otherwise looks like you have to manually install everything

## Ubuntu

### Update System
1. (Debian-based) `sudo apt-get update && sudo apt-get dist-upgrade -y`
2. Install essential tools `sudo apt-get install build-essential curl file git`
3. Set up zsh `sudo dnf install zsh`
	1. Install oh-my-zsh same as ubuntu
	2. Install `sudo dnf install powerline powerline-fonts`
	3. Clone autosuggestions/syntax highlighting per Ubuntu instructions as well 

### Set up ufw (while still as root, otherwise add `sudo`) on servers
1. `ufw app list` to see what's available
2. `ufw allow OpenSSH` for initial set-up, although we'll change this port later and need to update the firewall rules at that time
3. `ufw enable` and `ufw status` to make sure it's working
4. Log out and back in to make sure you haven't borked something and blocked yourself

### Add Non-Root User
1. `adduser eli`
2. give new user sudo permissions
    1. (option 1) `visudo` and add `eli ALL=(ALL) ALL`
    2. (option 2 - Ubuntu/Debian) `apt-get install sudo && usermod -a -G sudo eli` 

### Change to new user
1. Log out, and log back in using new user's pass to make sure it's working

### Configure Vim
1. `sudo apt install vim`
2. `vim --version`. If it's neovim, skip to step 4. Otherwise follow step 3.
3. (option 1) Update `~/.vimrc` to match [vimrc](vimrc)
    1. (optional) Remap CTRL key for keyboard to CAPS LOCK
      1. `sudo apt install gnome-tweak-tool`
      2. open tweak tool
      3. _Keyboard & Mouse_ > _Additional Layout Options_ > _Caps Lock Behavior_ > select _Caps Lock is also a Ctrl_
4.  (option 2) Update `$HOME/.config/nvim/init.vim` to match (or include) .vimrc for Neovim, which seems to be a default installation in Parrot OS
    1. More simply, just add `source ~/.vimrc` to this `init.vim` file after following previous instructions
    2. You can check which files are being checked upon opening vim via `:scriptnames`. If you don't see ~/.vimrc there's a good chance you're using neovim.       

### Configure CLI
1. install zsh, oh-my-zsh
    1. `sudo apt install zsh`
    2. `sudo apt install git-core curl fonts-powerline`
    3. `sh -c "$(curl -fsSL https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"`
2. Install zsh-autosuggestions
    1. `sudo git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions`  
3. Install zsh-syntax-highlighting
    1. `sudo git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting`  
4. use [zshrc file](zshrc) file in this directory and put it in ~/.zshrc
5. create `greeting.txt` in `$HOME` starting [here](http://www.patorjk.com/software/taag/#p=display&f=Doom&t=once%20more%0Ainto%20the%20%0Abreach)
6. `source .zshrc` to make sure it's working
7. Update default profile of terminal to start with zsh
    1. open terminal 
    2. go to `Edit` >  `Profile Preferences` > `Title and Command` > `Run a custom command instead of my shell` > Type `zsh` > Close
8. (optional) Copy over `.zsh_history` to `$HOME` and `chown [eli] .zsh_history`  
9. move espanso to __ folders (Not sure on Linux atm)

### Install Keepass
1. `sudo apt install keepassxc`
2. Import key database from other device

### Setting up SSH (for remote access devices only)
1. `ssh-copy-id -i id_rsa.pub eli@[host]`
2. Edit `/etc/ssh/sshd_config` to include
    1.  `PasswordAuthentication no`
    2. `Port [non-default port #]`
    3. `PermitRootLogin no`
    4. `AllowUsers eli`
5. `ufw allow [non-default port #]/tcp`
6. `sudo systemctl restart sshd`
7. Finally, log in via `ssh eli@[host] -p [non-default port #]

### Install Obsidian
1. Download [latest appimage](https://obsidian.md/download).
2. `mkdir ~/AppImages`
3. `mv [Obsidian.AppImage] ~/AppImages`
4. `chmod +x [Obsidian.AppImage]`
5. Add to menus (depends on OS)
    1. Use [this logo](obsidian-logo.png) for the thumbnail 
6. Open Obsidian
7. Create New Vault named `ElisSpace`
8. Go to `Core Plugins` and turn on `Sync`
9. Go to sync options and log in 
10. Go to sync options again, link to ElisSpace remote vault
11. DO NOT `Start Syncing` yet. First, turn on all options for syncing so _everything_ is synced.

### Configure burpsuite
1. install burpsuite
    1. download from [portswigger website](https://portswigger.net/burp/releases/community/latest)
    2. `chmod +x` [burpsuite file]
    3. `./[burpsuite file]`
    4. keep default options in installer 
3. add `export _JAVA_OPTIONS="-Dsun.java2d.uiScale=2"` after the shebang to the executable `$HOME\BurpSuiteCommunity\BurpSuiteCommunity` 

### Configure OpenVPN server and clients
1. Thanks [digital ocean](https://www.digitalocean.com/community/tutorials/how-to-set-up-and-configure-an-openvpn-server-on-ubuntu-20-04)

### Configure Screenshotting
1. `sudo apt install gnome-screenshot`
2. Create new entry in _keyboard shortcuts_ settings:
    1. Name: `Screenshot Selection and Copy to Clipboard`
    2. Command: `/usr/bin/gnome-screenshot -a -c`
3. Save, and double-click to set shortcut: `Shift + Mod4 + S`
---
# Windows
1. move espanso folders to `C:\\Users\\PC\\AppData\\Local\\espanso`
2. Right Click espanso icon > "Reload Config"
