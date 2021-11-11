#!/usr/bin/sh

# env variables
TMP_FILE=$(mktemp)
REMOTE_CONFIG_FILE_URL='https://raw.githubusercontent.com/husamuddin/vimrc/main/.vimrc'

makeNvimConfig () {
  # neovim config to read from .vimrc
  echo "  - make neovim reads from .vimrc"
  mkdir -p ~/.config/nvim
  cat <<EOT > ~/.config/nvim/init.vim
set runtimepath^=~/.vim runtimepath+=~/.vim/after
let &packpath = &runtimepath
source ~/.vimrc
EOT

  # Create .vimrc
  cp $TMP_FILE ~/.vimrc

  # Install Plug
  echo "  - installing plug plugin manager"
  mkdir -p ~/.vim/autoload 
  curl -LsS \
      https://raw.githubusercontent.com/junegunn/vim-plug/master/plug.vim \
      -o ~/.vim/autoload/plug.vim

  # Install Plugins
  echo "  - Install Plugins with plug"
  nvim +PlugInstall +qa
}

# download the remote config file
echo "> download the remote config"
curl -LsS $REMOTE_CONFIG_FILE_URL -o $TMP_FILE

if cmp -s "$HOME/.vimrc" "$TMP_FILE"
then
  echo "You are up to date"
else
  echo "> setting up configuration"
  makeNvimConfig
fi

# cleaning up
rm $TMP_FILE
