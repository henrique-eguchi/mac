# macOS Setup

Instructions to set up a Mac for development (based on [Tania Rascia setup](https://www.taniarascia.com/setting-up-a-brand-new-mac-for-development/))

## Settings

### Trackpad

- Point & Click -> Tap to click -> On

### Keyboard

- Key Repeat -> Fast
- Delay Until Repeat -> Short
- Disable "Correct spelling automatically"
- Disable "Capitalize words automatically"
- Disable "Add period with double-space"
- Disable "Use smart quotes and dashes"

### Network

- Firewall -> On

### Privacy & Security

- Allow applications from -> App Store & Known Developers

### Sharing

- File Sharing -> Off

### Login Items & Extensions

- AlDente


## Finder

### Show Library folder

```shell
chflags nohidden ~/Library
```

### Show hidden files

This can also be done by pressing `command` + `shift` + `.`.

```shell
defaults write com.apple.finder AppleShowAllFiles YES
```

### Show path bar

```shell
defaults write com.apple.finder ShowPathbar -bool true
```

### Show status bar

```shell
defaults write com.apple.finder ShowStatusBar -bool true
```

## Xcode

Install Xcode directly from App Store.

> If you just need compilers and unix-style utilities required for everyday development tasks, including running version control, compiling code, and using package managers like Homebrew and you don't need the full Xcode app, install the Xcode Command Line Tools by running `xcode-select --install`

## Homebrew
Package manager for operating system libraries and applications.

```shell
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

### Git

Git is already installed with either Xcode or just the Xcode Command Line Tools.

### GUI Applications

```shell
brew install --cask \
visual-studio-code \
google-chrome \
discord \
slack \
spotify \
notion \
aldente \
proxyman \
bruno \
ghostty \
vlc \
balenaetcher \
obs
```

### Misc

```shell
brew install blackhole-2ch
```

## Xcode configurations (optional)
### Track build time in Xcode
```shell
defaults write com.apple.dt.Xcode ShowBuildOperationDuration -bool YES
```
### Improve your Swift project build time
```shell
defaults write com.apple.dt.Xcode BuildSystemScheduleInherentlyParallelCommandsExclusively -bool NO
```
### Use Simulator in full-screen mode with Xcode
```shell
defaults write com.apple.iphonesimulator AllowFullscreenMode -bool YES
```
### Enable more secret features in Simulator using Apple hidden Internals menu
Create an empty folder with name “AppleInternal” in the root directory. Just run this command below and restart Simulator
```shell
sudo mkdir /AppleInternal
```
### Capture iOS Simulator video
```shell
xcrun simctl io booted recordVideo <filename>.<file extension>
```
Press control + c to stop recording the video. The default location for the created file is the current directory
### Use your fingerprint to sudo
Edit /etc/pam.d/sudo and add the following line to the top
```shell
auth sufficient pam_tid.so
```
### Remove unavailable simulators from Xcode
```shell
xcrun simctl delete unavailable
```

## GitHub

### Config - `~/.gitconfig`


```shell
[user]
	name = First Last
	email = email@email.com
[github]
	user = username
[alias]
	a = add
	ca = commit -a
	cam = commit -am
	s = status
	pom = push origin master
	pog = push origin gh-pages
	puom = pull origin master
	puog = pull origin gh-pages
	cob = checkout -b
[credential]
	helper = osxkeychain
```


## SSH

### Config - `~./ssh/config`

```shell
Host example
    HostName example.com
    User example-user
    IdentityFile key.pem
```

### Generate SSH key

```shell
ssh-keygen -t rsa -b 4096 -C "email@email.com"
```

## Bash

### Config - `~/.bash_profile`

touch ~/.bash_profile; nano ~/.bash_profile

```shell
PS1='$(networksetup -getcomputername):\W \u\$ '

alias brewup='brew update; brew upgrade; brew prune; brew cleanup; brew doctor'
export EDITOR="code -w"
```

```shell
source ~/.bash_profile
```

### Terminal Colors

```bash
export CLICOLOR=1
export LSCOLORS=ExFxBxDxCxegedabagacad
parse_git_branch() {
  git branch 2> /dev/null | sed -e '/^[^*]/d' -e 's/* \(.*\)/ (\1)/'
}
export PS1="\[\033[36m\]\u\[\033[m\]@\[\033[32m\]\h:\[\033[33;1m\]\w\$(parse_git_branch)\[\033[m\]\$ "
```
