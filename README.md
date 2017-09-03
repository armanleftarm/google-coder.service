# google-coder.service
A user unit file for systemd on Arch Linux ARM (ALARM) for google-coder. Much of this information is available across various tutorials. This intends to be a one-stop shop from desktop environment to Google-Coder.

This is assuming you have a fresh install of ALARM, with a DE/WM (I prefer LXDM & Fluxbox)

# Upgrade and install developer tools 
  
  pacman -Syu base-devel git

# Install Node Version Manager (NVM)
  curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.31.7/install.sh | bash

# Rerun Profile script to start NVM
  source ~/.bashrc  # Rerun profile after installing nvm
  
# Install Node.js using Node Version Manager
  
  nvm install 4  # Installs Node v4, (nvm install stable) installs Latest version of node
  
  nvm use 4  # Sets Node to use v4 (there's some breaking changes at some point beyond v4)

# Check versions before proceeding
  
  npm install npm@latest -g
  
  npm -v
  nvm --version
  node -v


# Get the coder source tree using the command line in home dir: 

  cd ~

  git clone https://github.com/googlecreativelab/coder

# Let's edit config.js, it's configured with Raspberry Pi in mind

  nano ~/coder/coder-base/config.js

# Replace the first group of lines with:

  exports.listenIP = '127.0.0.1';
  exports.listenPort = '8081';
  exports.httpListenPort = '8080';
  exports.cacheApps = true;
  exports.httpVisiblePort = '8080';
  exports.httpsVisiblePort = '8081';
  
# Install the script to coder-base 

  cd ~/coder-dist/coder-apps/
  
  ./install_common.sh ../coder-base/

# Test for working

  cd ../coder-base
  node server.js

# If at this point there are no errors, you should be able to access 127.0.0.1:8081 in your browser. 
# Ignore SSL warnings. In chrome, click Advanced -> Proceed Anyway

  mkdir ~/.config/systemd && mkdir ~/.config/systemd/user
  
# Here we can save the .service file

  cd ~/.config/systemd/user/ && git clone https://github.com/armanleftarm/google-coder.service
  system --user enable google-coder.service
  system --user start google-coder.service
  
# Finally reboot and check if working. If not, refer to 'journalctl -n100'
