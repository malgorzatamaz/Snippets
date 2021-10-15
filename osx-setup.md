# Setup Mac OS X

I've done the same process every couple years since 2013 (Mountain
Lion, Mavericks, High Sierra, Catalina) and I updated the Gist each time I've
done it.

I kinda regret for not using something like [Boxen](https://github.com/boxen/our-boxen/)
(or anything similar) to automate the process, but TBH I only actually needed to
these steps once every couple years...

This gist is just a personal reference in case I need to do it all over again.
**I'm by no means an OSX/\*nix expert, use with care.**

## Setup

### 1. Run software update

Make sure everything is up to date.

### 2. Install Xcode and/or "Command Line Tools"

> NOTE: homebrew now automatically installs command line tools, so you can skip this step.

"Command Line Tools" can be downloaded separate from Xcode at
https://developer.apple.com/downloads/ — It is way smaller than installing the
whole Xcode but might not work for all cases tho — or you can run `xcode-select --install`

Xcode can be found on App Store. **preferred**

More info on [how to download Command Line Tools inside XCode can be found on StackOverflow](http://stackoverflow.com/questions/9329243/xcode-4-4-and-later-install-command-line-tools)

### 3. Install homebrew and other CLI tools

http://brew.sh/

```sh
# install homebrew
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

brew install git
brew install node
brew install wget
brew install z
brew install ag
brew install ack
brew install fd
brew install ffind
brew install fpp
brew install tmux
brew install watchman
brew install yarn

npm install -g ipt
npm install -g http-server
npm install -g esformatter
npm install -g prettier
npm install -g eslint
# yeah, haters gonna hate
npm install -g replace
```

### 4. Install softwares

#### homebrew-cask

Many softwares can be installed through
[homebrew-cask](https://github.com/Homebrew/homebrew-cask) which makes the
process way simpler:

```sh
# essential
brew cask install dropbox
brew cask install 1password
brew cask install sensiblesidebuttons

# dev
brew cask install charles
brew cask install imagealpha
brew cask install imageoptim
brew cask install iterm2
brew cask install macvim

# utils
brew cask install divvy
# brew cask install the-unarchiver

# browsers
brew cask install firefox
brew cask install google-chrome

# others
# brew cask install limechat
# brew cask install skype
brew cask install rightzoom
# brew cask install vlc

# quick look plugins (https://github.com/sindresorhus/quick-look-plugins)
brew cask install qlcolorcode qlstephen qlmarkdown quicklook-json qlimagesize webpquicklook suspicious-package quicklookase qlvideo
```

#### App Store

- [Gifski](https://gif.ski/) (convert video into high-quality Gifs)
- [Amphetamine](https://itunes.apple.com/us/app/amphetamine/id937984704?mt=12) (prevent mac from sleeping)

#### Manually

- [Firefox Nightly](http://nightly.mozilla.org/)
- [Chrome Canary](https://www.google.com/intl/en/chrome/browser/canary.html) ([how to set canary as default browser](http://aaltonen.co/2011/05/06/make-chrome-canary-the-default-browser-on-mac-os-x/))
- Adobe Suite (Photoshop, Illustrator, ...)

### 5. Borrow a few OSX settings from [mathiasbynens dotfiles](https://github.com/mathiasbynens/dotfiles)

```sh
###############################################################################
# General UI/UX                                                               #
###############################################################################

# Disable the sound effects on boot
sudo nvram SystemAudioVolume=" "


###############################################################################
# Finder                                                                      #
###############################################################################

# Finder: show hidden files by default
defaults write com.apple.finder AppleShowAllFiles -bool true

# Finder: show all filename extensions
defaults write NSGlobalDomain AppleShowAllExtensions -bool true

# Finder: show status bar
defaults write com.apple.finder ShowStatusBar -bool true

# Finder: allow text selection in Quick Look
defaults write com.apple.finder QLEnableTextSelection -bool true

# Disable the warning when changing a file extension
defaults write com.apple.finder FXEnableExtensionChangeWarning -bool false

# Avoid creating .DS_Store files on network or USB volumes
defaults write com.apple.desktopservices DSDontWriteNetworkStores -bool true
defaults write com.apple.desktopservices DSDontWriteUSBStores -bool true

# Enable snap-to-grid for icons on the desktop and in other icon views
/usr/libexec/PlistBuddy -c "Set :DesktopViewSettings:IconViewSettings:arrangeBy grid" ~/Library/Preferences/com.apple.finder.plist
/usr/libexec/PlistBuddy -c "Set :FK_StandardViewSettings:IconViewSettings:arrangeBy grid" ~/Library/Preferences/com.apple.finder.plist
/usr/libexec/PlistBuddy -c "Set :StandardViewSettings:IconViewSettings:arrangeBy grid" ~/Library/Preferences/com.apple.finder.plist


###############################################################################
# Screen                                                                      #
###############################################################################

# Save screenshots to the desktop
defaults write com.apple.screencapture location -string "${HOME}/Desktop"

# Save screenshots in PNG format (other options: BMP, GIF, JPG, PDF, TIFF)
defaults write com.apple.screencapture type -string "png"

# Disable shadow in screenshots
defaults write com.apple.screencapture disable-shadow -bool true


###############################################################################
# Address Book, Dashboard, iCal, TextEdit, and Disk Utility                   #
###############################################################################

# Use plain text mode for new TextEdit documents
defaults write com.apple.TextEdit RichText -int 0
# Open and save files as UTF-8 in TextEdit
defaults write com.apple.TextEdit PlainTextEncoding -int 4
defaults write com.apple.TextEdit PlainTextEncodingForWrite -int 4


###############################################################################
# Time Machine                                                                #
###############################################################################

# Prevent Time Machine from prompting to use new hard drives as backup volume
defaults write com.apple.TimeMachine DoNotOfferNewDisksForBackup -bool true


###############################################################################
# Spotlight                                                                   #
###############################################################################

# Hide Spotlight tray-icon (and subsequent helper)
#sudo chmod 600 /System/Library/CoreServices/Search.bundle/Contents/MacOS/Search
# Disable Spotlight indexing for any volume that gets mounted and has not yet
# been indexed before.
# Use `sudo mdutil -i off "/Volumes/foo"` to stop indexing any volume.
sudo defaults write /.Spotlight-V100/VolumeConfiguration Exclusions -array "/Volumes"
# Change indexing order and disable some search results
# Yosemite-specific search results (remove them if you are using macOS 10.9 or older):
#   MENU_DEFINITION
#   MENU_CONVERSION
#   MENU_EXPRESSION
#   MENU_SPOTLIGHT_SUGGESTIONS (send search queries to Apple)
#   MENU_WEBSEARCH             (send search queries to Apple)
#   MENU_OTHER
defaults write com.apple.spotlight orderedItems -array \
  '{"enabled" = 1;"name" = "APPLICATIONS";}' \
  '{"enabled" = 1;"name" = "SYSTEM_PREFS";}' \
  '{"enabled" = 0;"name" = "DIRECTORIES";}' \
  '{"enabled" = 0;"name" = "PDF";}' \
  '{"enabled" = 0;"name" = "FONTS";}' \
  '{"enabled" = 0;"name" = "DOCUMENTS";}' \
  '{"enabled" = 0;"name" = "MESSAGES";}' \
  '{"enabled" = 0;"name" = "CONTACT";}' \
  '{"enabled" = 0;"name" = "EVENT_TODO";}' \
  '{"enabled" = 0;"name" = "IMAGES";}' \
  '{"enabled" = 0;"name" = "BOOKMARKS";}' \
  '{"enabled" = 0;"name" = "MUSIC";}' \
  '{"enabled" = 0;"name" = "MOVIES";}' \
  '{"enabled" = 0;"name" = "PRESENTATIONS";}' \
  '{"enabled" = 0;"name" = "SPREADSHEETS";}' \
  '{"enabled" = 0;"name" = "SOURCE";}' \
  '{"enabled" = 0;"name" = "MENU_DEFINITION";}' \
  '{"enabled" = 0;"name" = "MENU_OTHER";}' \
  '{"enabled" = 0;"name" = "MENU_CONVERSION";}' \
  '{"enabled" = 0;"name" = "MENU_EXPRESSION";}' \
  '{"enabled" = 0;"name" = "MENU_WEBSEARCH";}' \
  '{"enabled" = 0;"name" = "MENU_SPOTLIGHT_SUGGESTIONS";}'
# Load new settings before rebuilding the index
killall mds > /dev/null 2>&1
# Make sure indexing is enabled for the main volume
sudo mdutil -i on / > /dev/null
# Rebuild the index from scratch
sudo mdutil -E / > /dev/nule
```

source: https://github.com/mathiasbynens/dotfiles/blob/master/.macos

### 6. Configure zsh

Set zsh as defaut shell:

```sh
chsh -s /bin/zsh
```

Install [oh-my-zsh](https://ohmyz.sh/):

```sh
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

Edit the `~/.zshrc` file (after installing oh-my-zsh)

```sh
ZSH_THEME="hyperzsh"

plugins=(gitfast fd nvm vi-mode z)

alias ipt-lg="git log --pretty=format:'%h | %<(80)%s | %<(12)%cr | %<(20)%an | %d' -15 | ipt --size 15 | sed 's#^[^0-9a-z]*\([0-9a-z]*\).*#\1#' | xargs git checkout"
alias ipt-branch="git branch | ipt | xargs git checkout"
alias ipt-bdelete="git branch | ipt -m | xargs git branch -D"
alias ipt-vim="ipt -pm | xargs -o vim"
alias ipt-gvim="ipt -pm | xargs -o gvim --remote-silent"
```

I'm also using a tweaked [hyperzsh](https://github.com/tylerreckart/hyperzsh)
theme but I deleted the `_user_host` and `_python_venv` logic. I actually keep
it on my Dropbox folder and just symlink it:

```sh
ln -s ~/Dropbox/dotfiles/hyperzsh.zsh-theme ~/.oh-my-zsh/custom/themes/hyperzsh.zsh-theme
```

### 7. Create/Update `~/.gitconfig`

```gitconfig
; I removed the [user] and [github] blocks on purpose so other people don't copy it by mistake
; you will need to set these values later
[apply]
  whitespace = fix
[color]
  ui = auto
[color "branch"]
  current = yellow reverse
  local = yellow
  remote = green
[color "diff"]
  meta = yellow bold
  frag = magenta bold
  old = red bold
  new = green bold
[color "status"]
  added = yellow
  changed = green
  untracked = cyan
[merge]
  log = true
[push]
  ; "simple" avoid headaches, specially if you use `--force` w/o specifying branch
  ; see: http://stackoverflow.com/questions/13148066/warning-push-default-is-unset-its-implicit-value-is-changing-in-git-2-0
  default = simple
[url "git://github.com/"]
  insteadOf = "github:"
[url "git@github.com:"]
  insteadOf = "gh:"
  pushInsteadOf = "github:"
  pushInsteadOf = "git://github.com/"
[url "git@github.com:millermedeiros/"]
  insteadOf = "mm:"
[url "git@github.com:mout/"]
  insteadOf = "mout:"
[core]
  excludesfile = ~/.gitignore_global
  ; setting the editor fixes git commit bug http://tooky.co.uk/2010/04/08/there-was-a-problem-with-the-editor-vi-git-on-mac-os-x.html
  editor = /usr/bin/vim
[alias]
  ; show merge tree + commits info
  graph = log --graph --date-order -C -M --pretty=format:\"<%h> %ad [%an] %Cgreen%d%Creset %s\" --all --date=short
  lg = log --graph --pretty=format:'%Cred%h%Creset %C(yellow)%an%d%Creset %s %Cgreen(%cr)%Creset' --date=relative
  ; basic logging for quick browsing
  ls = log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cgreen\\ [%cn]" --decorate
  ll = log --pretty=format:"%C(yellow)%h%Cred%d\\ %Creset%s%Cgreen\\ [%cn]" --decorate --numstat
  ; log + file diff
  fl = log -u
  ; find paths that matches the string
  f = "!git ls-files | grep -i"
  ; delete all merged branches
  ; dm = !git branch --merged | grep -v "\*" | xargs -n 1 git branch -d
  ; shortcuts
  cp = cherry-pick
  st = status -s
  cl = clone
  ci = commit
  co = checkout
  br = branch
  dc = diff --cached
```

You will need to set the user name and email (removed from .gitconfig to avoid
errors):

```sh
git config --global user.name "Your Name Here"
git config --global user.email youremail@example.com
git config --global github.user yourGithubUserNameHere
```

### 8. Config vim

[my .vimrc is on this gist](https://gist.github.com/millermedeiros/1262085).

```sh
# create symlink to my vimrc file
ln -s ~/Dropbox/dotfiles/vimrc ~/.vimrc
# install Vundle to manage my plugins (https://github.com/VundleVim/Vundle.vim)
git clone https://github.com/VundleVim/Vundle.vim.git ~/.vim/bundle/Vundle.vim
```

### 9. Create/Update `~/.tmux.config`

```sh
# Smart pane switching with awareness of Vim splits.
# See: https://github.com/christoomey/vim-tmux-navigator
is_vim="ps -o state= -o comm= -t '#{pane_tty}' \
    | grep -iqE '^[^TXZ ]+ +(\\S+\\/)?g?(view|n?vim?x?)(diff)?$'"
bind-key -n C-h if-shell "$is_vim" "send-keys C-h"  "select-pane -L"
bind-key -n C-j if-shell "$is_vim" "send-keys C-j"  "select-pane -D"
bind-key -n C-k if-shell "$is_vim" "send-keys C-k"  "select-pane -U"
bind-key -n C-l if-shell "$is_vim" "send-keys C-l"  "select-pane -R"
bind-key -n C-\ if-shell "$is_vim" "send-keys C-\\" "select-pane -l"
bind-key -T copy-mode-vi C-h select-pane -L
bind-key -T copy-mode-vi C-j select-pane -D
bind-key -T copy-mode-vi C-k select-pane -U
bind-key -T copy-mode-vi C-l select-pane -R
bind-key -T copy-mode-vi C-\ select-pane -l

# refresh 'status-left' and 'status-right' more often
set-option -g status-interval 5

# increase scrollback buffer size
set-option -g history-limit 50000

# address vim mode switching delay (http://superuser.com/a/252717/65504)
set-option -s escape-time 0

# focus events enabled for terminals that support them
set-option -g focus-events on

bind-key C-p previous-window
bind-key C-n next-window

# enable mouse
setw -g mouse on

# set default shell to zsh
set -g default-command /bin/zsh
set -g default-shell /bin/zsh
```

I actually keep this on my Dropbox folder and just symlink it:

```sh
ln -s ~/Dropbox/dotfiles/tmux.config ~/.tmux.config
```

TODO: borrow tmux settings from [@ruyadorno](https://github.com/ruyadorno/installme-osx/)

### 10. Configure npm and generate SSH keys for github

Need to set the npm user:

```sh
npm adduser
```

And also [generate SSH keys for github](https://help.github.com/articles/generating-ssh-keys)

### 11. Copy stuff from old HD

```sh
# recursively copy files and folders
# beware of rsync `-C, --cvs-exclude` flag since it might exclude files you
# don't want to like *.exe, core, tags...
rsync -av '/Volumes/Macintosh HD/Users/millermedeiros/Projects' ~/tmp_projects
rsync -av '/Volumes/Macintosh HD/Users/millermedeiros/Music/iTunes/iTunes Media' ~/tmp_music
```

Check if files were copied properly and rename/move. Copying to a temporary
folder since `rsync` might delete files depending on the options and/or merge
folders that you do not want to merge.

`rsync` is great, you should use it when possible.

### 12. Profit!
