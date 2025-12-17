# 🚀 Ultimate Workstation Setup Guide
**Systems:** Fedora (Primary) & Ubuntu (Shell Support)
**Shell:** Zsh + Oh-My-Zsh + Powerlevel10k + fzf (Tab-enabled)



## 1️⃣ Shell Environment Setup
### 1. Install Zsh & Utilities

**Fedora:**
```bash
sudo dnf install zsh fzf util-linux-user -y

```

**Ubuntu:**

```bash
sudo apt update && sudo apt install zsh fzf -y

```

### 2. Set Zsh as Default

```bash
chsh -s /bin/zsh
# Note: A logout/login is required for this to take full effect, but you can proceed with the setup.

```

### 3. Configure Directory Hygiene (ZDOTDIR)

Keep your home directory clean by forcing Zsh config into `~/.config/zsh`.

```bash
mkdir -p ~/.config/zsh

# Create the loader file in Home
cat > ~/.zshrc << 'EOF'
export ZDOTDIR="$HOME/.config/zsh"
source "$ZDOTDIR/.zshrc"
EOF

```

### 4. Install Fonts (FiraMono Nerd Font)

Required for Powerlevel10k icons.

**Method A: Package Manager**

* **Fedora:** `sudo dnf install fira-code-fonts fira-mono-fonts -y`
* **Ubuntu:** `sudo apt install fonts-firacode -y`

**Method B: Manual (Recommended for full icon support)**

```bash
mkdir -p ~/.local/share/fonts
cd ~/.local/share/fonts
wget [https://github.com/ryanoasis/nerd-fonts/releases/latest/download/FiraMono.zip](https://github.com/ryanoasis/nerd-fonts/releases/latest/download/FiraMono.zip)
unzip FiraMono.zip
fc-cache -fv
# ⚠️ Action: Set "FiraMono Nerd Font" in your terminal settings.

```

### 5. Install Oh-My-Zsh & Plugins

Clone these into the `ZDOTDIR` structure.

**Oh-My-Zsh:**

```bash
git clone [https://github.com/ohmyzsh/ohmyzsh.git](https://github.com/ohmyzsh/ohmyzsh.git) ~/.config/zsh/oh-my-zsh

```

**Powerlevel10k Theme:**

```bash
git clone --depth=1 [https://github.com/romkatv/powerlevel10k.git](https://github.com/romkatv/powerlevel10k.git) \
  ~/.config/zsh/oh-my-zsh/custom/themes/powerlevel10k

```

**Plugins (Autosuggestions, Syntax Highlighting, fzf-tab):**

```bash
git clone [https://github.com/zsh-users/zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) \
  ~/.config/zsh/oh-my-zsh/custom/plugins/zsh-autosuggestions

git clone [https://github.com/zsh-users/zsh-syntax-highlighting.git](https://github.com/zsh-users/zsh-syntax-highlighting.git) \
  ~/.config/zsh/oh-my-zsh/custom/plugins/zsh-syntax-highlighting

git clone [https://github.com/Aloxaf/fzf-tab](https://github.com/Aloxaf/fzf-tab) \
  ~/.config/zsh/oh-my-zsh/custom/plugins/fzf-tab

```

### 6. Install FZF (Latest Script)

Ensures you have the latest binary features.

```bash
git clone --depth 1 [https://github.com/junegunn/fzf](https://github.com/junegunn/fzf) ~/.local/share/fzf
~/.local/share/fzf/install --no-bash --no-fish
mv ~/.fzf.zsh ~/.config/zsh/

```

---

## 2️⃣ The Configuration File (`.zshrc`)

Run this block to create your production config. **This includes the specific settings to make TAB trigger fzf.**

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

**Finalize Shell:**

```bash
exec zsh
# Follow the Powerlevel10k configuration prompts.

```

---

## 3️⃣ System & BIOS Maintenance (Fedora/Ubuntu)

### Update BIOS / Firmware

**Warning:** Ensure your laptop is plugged into power before running these.

```bash
# 1. Refresh firmware metadata from LVFS
sudo fwupdmgr refresh

# 2. Check for available updates
sudo fwupdmgr get-updates

# 3. Install updates (System will likely prompt to reboot)
sudo fwupdmgr update

```

### Desktop Customization (GNOME)

```bash
# Set Window Controls (Minimize, Maximize, Close)
gsettings set org.gnome.desktop.wm.preferences button-layout ':minimize,maximize,close'

# Install Tweaks & Extensions Tool
sudo dnf install gnome-tweaks gnome-extensions-app -y

```

---

## 4️⃣ Application Provisioning (Fedora)

### System Utilities

```bash
sudo dnf install dnf-plugins-core git -y

# Snap Support (Required for Postman)
sudo dnf install snapd -y
sudo ln -s /var/lib/snapd/snap /snap
# Start Snap service
sudo systemctl enable --now snapd.socket

```

### Web Browsers

```bash
# Brave
curl -fsS https://dl.brave.com/install.sh | sh

# Google Chrome
sudo dnf install [https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm] https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm -y

```

### Developer Tools

```bash
# Visual Studio Code
sudo rpm --import [https://packages.microsoft.com/keys/microsoft.asc](https://packages.microsoft.com/keys/microsoft.asc)
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=[https://packages.microsoft.com/yumrepos/vscode](https://packages.microsoft.com/yumrepos/vscode)\nenabled=1\ngpgcheck=1\ngpgkey=[https://packages.microsoft.com/keys/microsoft.asc] https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'
sudo dnf check-update
sudo dnf install code -y

# Postman (via Snap)
sudo snap install postman

```

### Productivity & Tools

```bash
# Media & Office
sudo dnf install vlc libreoffice -y

# Security & Docs
sudo dnf install keepassxc zeal -y

# Disk Management
sudo dnf install gparted -y
sudo dnf install snapper snapper-gui -y

```
