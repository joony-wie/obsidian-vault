```bash
sudo dnf -y update
sudo dnf -y install epel-release

# Install i3
sudo dnf -y config-manager --set-enabled powertools
sudo dnf -y install https://pkgs.dyn.su/el8/base/x86_64/raven-release-1.0-1.el8.noarch.rpm
sudo dnf -y install raven-release
sudo dnf -y install i3

# Install font
sudo dnf -y install fontawesome-fonts fontawesome-fonts-web
mkdir $HOME/.local/share/fonts
wget "https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS NF Regular.ttf" -P $HOME/.local/share/fonts
wget "https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS NF Bold.ttf" -P $HOME/.local/share/fonts
wget "https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS NF Italic.ttf" -P $HOME/.local/share/fonts
wget "https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS NF Bold Italic.ttf" -P $HOME/.local/share/fonts
fc-cache -f -v
exit # open new terminal

# Install zsh
sudo dnf -y install zsh
chsh -s `which zsh`
echo $SHELL

# Install oh-my-zsh
curl -L https://raw.github.com/robbyrussell/oh-my-zsh/master/tools/install.sh | sh

# Install powerlevel10k
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
sed -i 's/robbyrussell/powerlevel10k\/powerlevel10k/g' .zshrc
source .zshrc

# Install zsh plugins
# zsh-autosuggestions
git clone [https://github.com/zsh-users/zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
sudo dnf -y install autojump-zsh
sed -i 's/plugins=(git)/plugins=(git zsh-autosuggestions zsh-syntax-highlighting autojump copypath dirhistory jsontools)/g' .zshrc

# Install LunarVim
sudo dnf -y install neovim
sudo dnf -y install gcc gdb cmake
sudo dnf -y install python3-devel
sudo dnf -y install cargo
LV_BRANCH='release-1.2/neovim-0.8' bash <(curl -s https://raw.githubusercontent.com/lunarvim/lunarvim/fc6873809934917b470bff1b072171879899a36b/utils/installer/install.sh)
# export PATH
echo 'export PATH=$HOME/.local/bin:$PATH' >> $HOME/.zshenv

# Install keyd for key remapping
mkdir -p $HOME/workspaces/git
cd $HOME/workspaces/git
git clone https://github.com/rvaiya/keyd
cd keyd
make && sudo make install
sudo systemctl enable keyd && sudo systemctl start keyd
sudo tee -a /etc/keyd/default.conf > /dev/null << EOF
[ids]

*

[main]

# Maps capslock to escape when pressed and control when held.
capslock = overload(control, esc)
EOF
sudo keyd reload

# Install alacritty terminal
sudo dnf -y install freetype-devel fontconfig-devel libxcb-devel libxkbcommon-devel
sudo dnf -y group install "Development Tools"
cargo install alacritty
# export PATH
echo 'export PATH=$HOME/.cargo/bin:$PATH' >> $HOME/.zshenv
```