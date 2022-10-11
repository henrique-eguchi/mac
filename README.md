# macOs Setup
My basic list of instructions to make setting up an Apple computer for iOS development (based on [Tania Rascia setup](https://www.taniarascia.com/setting-up-a-brand-new-mac-for-development/))

## System Preferences

- Trackpad > Point & Click > Tap to click > On
- Keyboard > Text > Disable "Correct spelling automatically".
- Security and Privacy > Firewall > On
- Security and Privacy > General > App Store and identified developers
- Sharing > File Sharing > Off
- Users & Groups > Login Items > Spectacle / AlDente

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

## Homebrew
For managing operating system libraries.

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### Mac App Store (command-line)

```shell
brew install mas
```

#### Sign in

```shell
mas signin email@email.com
```

### Brewfile

```shell
touch Brewfile
```

```shell
tap 'caskroom/cask'

brew 'git'

cask 'google-chrome'
cask 'spectacle'
cask 'visual-studio-code'

# mas 'Slack', id: 803453959
# mas 'Sip', id: 507257563 ##paid
mas 'Xcode', id: 497799835

## Applications that are not installable by cask
# OneDrive
# Office
```

```shell
brew bundle
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
