# macOs High Sierra v10.13 Setup
My basic list of instructions to make setting up an Apple computer for ios development (based on [Tania Rascia setup](https://www.taniarascia.com/setting-up-a-brand-new-mac-for-development/))

## Preferences

- **Keyboard > Text >** Disable "Correct spelling automatically".
- **Security and Privacy > Firewall >** On
- **Security and Privacy > General >** App Store and identified developers
- **File Sharing >** Off
- **Users & Groups > Login Items >** Spectacle

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

```shell
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

### Mac App Store

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
brew 'npm'

cask 'firefox'
cask 'google-chrome'
cask 'spectacle'
cask 'visual-studio-code'

# mas 'Slack', id: 803453959
# mas 'Sip', id: 507257563 ##paid
mas 'Xcode', id: 497799835

## Applications that are not installable by cask
# Firefox Developer Edition
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

```shell
alias brewup='brew update; brew upgrade; brew prune; brew cleanup; brew doctor'
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
