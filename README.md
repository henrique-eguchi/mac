# macOs High Sierra v10.13 Setup
My basic list of instructions to make setting up an Apple computer for ios and android development (based on [Tania Rascia setup](https://www.taniarascia.com/setting-up-a-brand-new-mac-for-development/))

## Preferences

- **Trackpad > Point & Click > Tap to click >** On
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

brew 'git' #version control
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
# OneDrive
# Office
# Microsoft Remote Desktop
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

### Programs that I will probably test
CodeKit (Front End Toolbox)

Tower (Git Manager)

Transmit (FTP program)


# Android Development Environment

### Brewfile
```bash
brew cask install java # /Library/Java/JavaVirtualMachines/jdk1.8.0_102.jdk/Contents/Home/ 
# In case of other version of java:
# brew cask install java7 # /Library/Java/JavaVirtualMachines/jdk1.7.0_80.jdk/Contents/Home/ 
# brew cask install java6 # /Library/Java/JavaVirtualMachines/jdk1.6.0_65.jdk/Contents/Home/
brew install jenv # To configure java installations easily
```

## Bash

### Config - `~/.bash_profile`

To configure jenv, add this to the .bash_profile file

```bash
if which jenv > /dev/null; then eval "$(jenv init -)"; fi
```

## jenv configuration

Reinitialize terminal and then run the following commands:

```bash
jenv add /Library/Java/JavaVirtualMachines/jdk1.6.0_65.jdk/Contents/Home/
jenv add /Library/Java/JavaVirtualMachines/jdk1.7.0_80.jdk/Contents/Home/
jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0_102.jdk/Contents/Home/
```

The following command will show all registered versions:

```bash
jenv versions
```

Tell which jdk version is the main:

```bash
jenv global oracle64-1.8.0.102
java -version #test it!
```

## Gradle

### sdkman (sdk package manager)

```bash
curl -s "https://get.sdkman.io" | bash
```

And then:

```bash
source "$HOME/.sdkman/bin/sdkman-init.sh"
sdk version
```

### Gradle installation

```bash
sdk install gradle
gradle -v
```

### Gradle configuration

```bash
touch ~/.gradle/gradle.properties
# insert the following:
echo 'org.gradle.daemon=true' >> ~/.gradle/gradle.properties
echo 'org.gradle.parallel=true' >> ~/.gradle/gradle.properties
echo 'org.gradle.jvmargs=-Xmx2048M' >> ~/.gradle/gradle.properties
```

## Android Studio

### Brewfile

```bash
brew cask install android-studio
```

### Android Studio configuration

```bash
vi /Applications/Android\ Studio.app/Contents/bin/studio.vmoptions
# insert the following:
-Xms2048m
-Xmx3072m
-XX:MaxPermSize=512m
-XX:ReservedCodeCacheSize=512m
-XX:+UseCompressedOops
```

Open the Android Studio and then configure it to use the Gradle:

Android Studio > Preferences > Build, Execution, Deployment > Gradle

Set the path to the GRADLE_HOME environment variable, generally at the user root folder:

/Users/<user>/.sdkman/candidates/gradle/current
