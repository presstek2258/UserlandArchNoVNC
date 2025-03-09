# UserlandArchNoVNC
This is just a readme of commands i used for my userland arch setup. There is no guaranteed that these command won't harm your system or delete files. Use at your own risk.  
Note: please use on fresh install to prevent overwritting.  

# userland arch setup

- This is for setting up a arch install on userland

> WARNING: only use on a fresh install. Data will be OVERWRITTEN!

## first update and add terminal colours

```bash
sudo pacman -Syu
# uncomment #Color in:
# add "ILoveCandy" in:
sudo nano /etc/pacman.conf
```

## fix yay

```bash
sudo pacman -S git go
sudo pacman --needed base-devel
git clone https://aur.archlinux.org/yay.git
cd yay
# makepkg will not install but will build the package
makepkg -si
```

```bash
# install with (might need to change to package name):
sudo pacman -U --noconfirm yay_built_package.pkg.tar.xz
```

## install other packages with yay

```bash
yay -S alacritty fzf neovim tmux npm i3-wm jdk-openjdk man-db neofetch novnc python-numpy rclone tigervnc tldr ttf-jetbrains-mono xclip xterm
```

## get nvim config

```bash
git clone https://github.com/presstek2258/presstek2258-nvim-tmux-dotfiles.git ~/temp-config
rm -rf ~/temp-config/.git
sudo cp -r ~/temp-config/. ~/
rm -rf ~/temp-config
```

## bash scripts for setup

```bash
# delRCloneLockFile.sh
# delete the rclone file (rclone entry name: pcloud)
rclone deletefile /home/userland/.cache/rclone/bisync/home_userland_pCloudDesktop..pcloud_pCloud_Desktop.lck
```

```bash
# runNoVNC.sh
novnc --vnc localhost:5901
```

```bash
# runPCloud.sh
# sync a folder called "pCloudDesktop"
rclone bisync ~/pCloudDesktop/ "pcloud:/pCloud Desktop" --resync
```

```bash
# runVNC.sh
vncserver :1
```

```bash
# runALL.sh
tmux new-session -d -s my_session
tmux split-window -h -t my_session
tmux split-window -v -t my_session:0.1
tmux send-keys -t my_session:0.0 "./runVNC.sh" C-m
tmux send-keys -t my_session:0.1 "./runPCloud.sh" C-m
tmux send-keys -t my_session:0.2 "./runNoVNC.sh" C-m
tmux attach-session -t my_session
```
