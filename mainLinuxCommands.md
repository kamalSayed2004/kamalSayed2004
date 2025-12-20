# This is a Linux guide for important scripts

## installing zsh with utilities

```bash
sudo dnf install zsh fzf zsh-autosuggestions zsh-syntax-highlighting util-linux-user -y

```

```bash
chsh -s /bin/zsh
# Note: A logout/login is required for this to take full effect, but you can proceed with the setup.

```

```bash
mkdir -p ~/.config/zsh

# Create the loader file in Home
cat > ~/.zshrc << 'EOF'
export ZDOTDIR="$HOME/.config/zsh"
source "$ZDOTDIR/.zshrc"
EOF

```

```bash
cat > ~/.config/zsh/.zshrc << 'EOF'
# --- Plugins & Tools ---
# Load Syntax Highlighting (must be last in most cases)
source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

# Load Autosuggestions
source /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh

# Load FZF (Fuzzy Finder)
source /usr/share/fzf/shell/key-bindings.zsh
source /usr/share/fzf/shell/completion.zsh

# --- History Configuration ---
HISTFILE="$ZDOTDIR/.zsh_history"
HISTSIZE=10000
SAVEHIST=10000
setopt appendhistory

# --- Key Bindings ---
# Use Up/Down arrows to search history based on what you've already typed
bindkey '^[[A' up-line-or-search
bindkey '^[[B' down-line-or-search

# --- Aliases ---
alias ll='ls -alF'
alias g='git'
EOF
```

```bash
source ~/.zshrc
```

## download MesloLGS NF, enter the directory after extraction then do the next command:

```bash
# Create the directory
sudo mkdir -p /usr/share/fonts/MesloLGS

# Download the 4 font variants directly into the directory
sudo curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Regular.ttf -o /usr/share/fonts/MesloLGS/MesloLGS_NF_Regular.ttf
sudo curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold.ttf -o /usr/share/fonts/MesloLGS/MesloLGS_NF_Bold.ttf
sudo curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Italic.ttf -o /usr/share/fonts/MesloLGS/MesloLGS_NF_Italic.ttf
sudo curl -L https://github.com/romkatv/powerlevel10k-media/raw/master/MesloLGS%20NF%20Bold%20Italic.ttf -o /usr/share/fonts/MesloLGS/MesloLGS_NF_Bold_Italic.ttf

# Update the system font cache
sudo fc-cache -f -v

```
## install ohmyzsh

```bash
ZDOTDIR=~/.config/zsh sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" --unattended

```
## install powerlevel10k and fzf-tab

```bash
# Clone Powerlevel10k
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/.config/zsh/ohmyzsh/custom/themes/powerlevel10k

# Clone fzf-tab
git clone https://github.com/Aloxaf/fzf-tab ~/.config/zsh/ohmyzsh/custom/plugins/fzf-tab
```

## open you configuration directory

```bash
nano ~/.config/zsh/.zshrc
```

## update the theme to powerlevel10k

```bash
ZSH_THEME="powerlevel10k/powerlevel10k"

```

## source all the plugins by adding this at the end of the file

```bash
# --- External Plugins & Tools ---

# Load FZF (Fuzzy Finder) system-wide integrations
source /usr/share/fzf/shell/key-bindings.zsh
source /usr/share/fzf/completion.zsh

# Load Autosuggestions
source /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh

# Load Syntax Highlighting (Must be the last plugin sourced)
source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.z>

# --- History Configuration ---
HISTFILE="$ZDOTDIR/.zsh_history"
HISTSIZE=10000
SAVEHIST=10000
setopt appendhistory

# --- Key Bindings ---
bindkey '^[[A' up-line-or-search
bindkey '^[[B' down-line-or-search

# --- Aliases ---
alias ll='ls -alF'
alias g='git'

# To customize Powerlevel10k, run `p10k configure` or edit ~/.p10k.>
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh

```

## updating the bios

**Warning:** Ensure your laptop is plugged into power before running these.

```bash
# 1. Refresh firmware metadata from LVFS
sudo fwupdmgr refresh

# 2. Check for available updates
sudo fwupdmgr get-updates

# 3. Install updates (System will likely prompt to reboot)
sudo fwupdmgr update

```

## installing gnome tweaks and settings windows buttons (maximize,minimize,close) and scroll speed for trackpad

```bash
# Set Window Controls (Minimize, Maximize, Close)
gsettings set org.gnome.desktop.wm.preferences button-layout ':minimize,maximize,close'

# Install Tweaks
sudo dnf install gnome-tweaks -y

# Set Scroll Speed
gsettings set org.gnome.desktop.peripherals.touchpad speed 0.5

```

## installing core plugins, git, snap, lsd

```bash
sudo dnf install dnf-plugins-core git -y

# Snap Support (Required for Postman)
sudo dnf install snapd -y
sudo ln -s /var/lib/snapd/snap /snap
# Start Snap service
sudo systemctl enable --now snapd.socket

sudo dnf install lsd

```

## installing web browsers (brave, chrome)

```bash
# Brave
curl -fsS https://dl.brave.com/install.sh | sh

# Google Chrome
sudo dnf install [https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm] https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm -y

```

## installing vs code and postman

```bash
# Visual Studio Code
sudo rpm --import [https://packages.microsoft.com/keys/microsoft.asc] https://packages.microsoft.com/keys/microsoft.asc
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=[https://packages.microsoft.com/yumrepos/vscode](https://packages.microsoft.com/yumrepos/vscode)\nenabled=1\ngpgcheck=1\ngpgkey=[https://packages.microsoft.com/keys/microsoft.asc] https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
sudo dnf check-update
sudo dnf install code -y

# Postman (via Snap)
sudo snap install postman

```

## installing gparted, libreoffice, keepass, snapper, zeal, planify

```bash
# Media & Office
sudo dnf install vlc libreoffice -y

# Security & Docs
sudo dnf install keepassxc zeal -y

# Disk Management
sudo dnf install gparted -y

# Snapper (alternative for TimeShift)
sudo dnf install snapper python3-dnf-plugin-snapper

# Snapper GUI
sudo dnf install btrfs-assistant

# Planify
sudo flatpak install io.github.alainm23.planify

```

## Adding an App to Apps Menu for example Zeal

```bash
nano ~/.local/share/applications/zeal.desktop

```

[Desktop Entry]
Name=Zeal
Comment=Offline Documentation Browser
Exec=zeal
Icon=zeal
Type=Application
Categories=Development;Office;
Terminal=false
StartupNotify=true

```bash
update-desktop-database ~/.local/share/applications
```

## install antigravity

```bash
sudo tee /etc/yum.repos.d/antigravity.repo << EOL
[antigravity-rpm]
name=Antigravity RPM Repository
baseurl=https://us-central1-yum.pkg.dev/projects/antigravity-auto-updater-dev/antigravity-rpm
enabled=1
gpgcheck=0
EOL
```

```bash
sudo dnf makecache
sudo dnf install antigravity
```

## install extension manager for gnome

```bash
sudo flatpak install com.mattjakeman.ExtensionManager
```

## some plugins for extension manager

```bash
blur my shell
```

```bash
burn my windows
```

```bash
caffeine
```

```bash
clipboard indicator
```

```bash
coverflow alt-tab
```

```bash
dash2dock animated
```

```bash
gsconnect
```

```bash
just perfection
```

```bash
tiling shell
```

```bash
user themes
```

```bash
vitals
```

```bash
wallpaper slideshow
```


## some apps for windows simulation

```bash
sudo dnf install lutris wine winetricks steam
sudo flatpak install net.davidotek.pupgui2
```
