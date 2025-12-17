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

## download fira fonts, enter the directory after extraction then do the next command:

```bash
sudo mkdir -p /usr/share/fonts/fira
sudo cp *.ttf /usr/share/fonts/fira/
sudo fc-cache -f -v

```

```bash
ZDOTDIR=~/.config/zsh sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" --unattended

```

```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ~/.config/zsh/oh-my-zsh/custom/themes/powerlevel10k

```

```bash
git clone https://github.com/Aloxaf/fzf-tab ~/.config/zsh/oh-my-zsh/custom/plugins/fzf-tab

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
source /usr/share/fzf/shell/completion.zsh

# Load Autosuggestions
source /usr/share/zsh-autosuggestions/zsh-autosuggestions.zsh

# Load Syntax Highlighting (Must be the last plugin sourced)
source /usr/share/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh

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

# To customize Powerlevel10k, run `p10k configure` or edit ~/.p10k.zsh.
[[ ! -f ~/.p10k.zsh ]] || source ~/.p10k.zsh
```

```bash
cat > ~/.config/zsh/.zshrc << 'EOF'
# --- SYSTEM PATHS ---
export ZDOTDIR="$HOME/.config/zsh"
export ZSH="$ZDOTDIR/oh-my-zsh"

# --- THEME ---
ZSH_THEME="powerlevel10k/powerlevel10k"

# --- PLUGINS ---
plugins=(
  git
  zsh-autosuggestions
  zsh-syntax-highlighting
  fzf-tab
)

# --- LOAD OH-MY-ZSH ---
source $ZSH/oh-my-zsh.sh

# --- FZF CONFIGURATION (SYSTEM) ---
# Load system key bindings if they exist
[ -f /usr/share/fzf/shell/key-bindings.zsh ] && source /usr/share/fzf/shell/key-bindings.zsh
[ -f /usr/share/fzf/shell/completion.zsh ] && source /usr/share/fzf/shell/completion.zsh

# --- FZF CONFIGURATION (USER) ---
# Load local fzf if installed via git
[ -f "$ZDOTDIR/.fzf.zsh" ] && source "$ZDOTDIR/.fzf.zsh"

# Default Style
export FZF_DEFAULT_OPTS="--height=40% --layout=reverse --border"

# --- FZF-TAB CONFIGURATION (TAB TRIGGERS) ---
# Disable default fzf completion trigger to avoid conflicts
export FZF_COMPLETION_TRIGGER=''

# Force fzf-tab to override standard completion
# Use 'cat' for preview (fastest). Use 'less' or 'bat' if preferred.
zstyle ':fzf-tab:complete:*:*' fzf-preview 'cat $realpath'
zstyle ':fzf-tab:complete:*:*' fzf-flags --height=40% --layout=reverse --border

# Switch result groups using comma and period
zstyle ':fzf-tab:*' switch-group ',' '.'

# --- FINAL LOAD ---
# Syntax highlighting must be loaded last
source $ZSH/custom/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh
EOF

```

```bash
exec zsh
# Follow the Powerlevel10k configuration prompts.

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

## installing gnome tweaks and settings windows buttons (maximize,minimize,close)

```bash
# Set Window Controls (Minimize, Maximize, Close)
gsettings set org.gnome.desktop.wm.preferences button-layout ':minimize,maximize,close'

# Install Tweaks
sudo dnf install gnome-tweaks -y

```

## installing core plugins, git, snap

```bash
sudo dnf install dnf-plugins-core git -y

# Snap Support (Required for Postman)
sudo dnf install snapd -y
sudo ln -s /var/lib/snapd/snap /snap
# Start Snap service
sudo systemctl enable --now snapd.socket

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

## installing gparted, libreoffice, keepass, snapper, zeal

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
