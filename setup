#!/bin/sh

echo "------------------------------"
echo "1) Setup Chromium"
echo "2) Setup Flatpak"
echo "3) Setup Neovim"
echo "4) Setup Zsh"
echo "5) Setup all"
echo "------------------------------"

echo "Choose an option: "
read -r option

configure_chromium() {
  CONFIG_DIR="$HOME/.var/app/org.chromium.Chromium/config"

  mkdir -p "$CONFIG_DIR"
  cat > "$CONFIG_DIR/chromium-flags.conf" << EOF
--force-dark-mode
--enable-features=WebUIDarkMode
--ozone-platform-hint=auto
EOF
}

setup_flatpak() {
  flatpak remote-add --if-not-exists flathub "https://flathub.org/repo/flathub.flatpakrepo"

  flatpak install org.kde.ark \
                  org.chromium.Chromium \
                  org.kde.elisa \
                  org.kde.gwenview \
                  org.kde.kdevelop \
                  org.kde.kid3 \
                  org.kde.kile \
                  org.kde.kwrite \
                  org.kde.kontact \
                  org.kde.okular \
                  io.mpv.Mpv

  flatpak override --user --socket=pcsc org.chromium.Chromium
  flatpak override --user --env=QT_QPA_PLATFORM=wayland org.kde.kontact
  flatpak override --user --env=XCURSOR_PATH=/run/host/user-share/icons:/run/host/share/icons
}

setup_nvim() {
  NVIM_DIR="$HOME/.config/nvim"
  NVIM_GIT="https://github.com/notfirefox/init.lua.git"

  if [ ! -f "$NVIM_DIR/init.lua" ]; then
    mkdir -p "$NVIM_DIR"
    git clone "$NVIM_GIT" "$NVIM_DIR"
  fi
}

setup_zsh() {
  ZSH_DIR="$HOME/.config/zsh"
  ZSH_GIT="https://github.com/notfirefox/init.zsh.git" 

  if [ ! -f "$ZSH_DIR/init.zsh" ]; then
    mkdir -p "$ZSH_DIR"
    git clone "$ZSH_GIT" "$ZSH_DIR"
    git -C "$ZSH_DIR" submodule update --init --recursive
  fi

  echo "source $ZSH_DIR/init.zsh" > "$HOME/.zshrc"
}

if [ "$option" -eq 1 ]; then
  configure_chromium
elif [ "$option" -eq 2 ]; then
  setup_flatpak
elif [ "$option" -eq 3 ]; then
  setup_nvim
elif [ "$option" -eq 4 ]; then
  setup_zsh
elif [ "$option" -eq 5 ]; then
  configure_chromium
  setup_flatpak
  setup_nvim
  setup_zsh
else
  echo "Exiting..."
fi
