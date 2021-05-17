# Install Jenkins node for Android/iOS on Mac OS

Ultimate walkthrough for setting up a Jenkins slave on Mac OS for iOS and Android development (and more).

## Mac OS setup

- Recommended hardware is MacMini (min. i7, 16GB RAM, 256 SSD) or Mac Pro.
- Clean OS install, also update to latest Mac OS.
- Mac is accessible from fix IP address or even domain address.
- Proxy setup if required (http_proxy, https_proxy environment variable is set correctly, proxy is added in network config).
  Mac OS usually has troubles with proxies that require authentications.
- Clean-up desk: keep only System Preferences, Safari, Finder and Terminal after install. Terminal needs to be added to desk.
- Prepare all technical users required accessing other services, like Git, Artifactory, Sonarqube etc.
- Connectivity: use Ethernet interface, turn off Wi-Fi and Bluetooth (if wireless Mouse & Keyboard not connected).
- Energy saving settings: enable "prevent sleeping when display is off", enable "wake for "Wi-Fi access", disable "power nap"
- Mobile phones (Android, iOS) connected to computer, with Black Screen app installed.

## IDEs and Core SDKs

### Xcode

https://apps.apple.com/us/app/xcode/id497799835?mt=12 
Start Xcode, install command line tools: `xcode-select --install`

### Android Studio + JAVA

Install Android Studio, with built in JDK.

Download link: https://developer.android.com/studio

Environment setup:

```sh
echo 'export ANDROID_HOME=/Users/$USER/Library/Android/sdk' >> ~/.bash_profile
echo 'export PATH=${PATH}:$ANDROID_HOME/tools:$ANDROID_HOME/platform-tools' >> ~/.bash_profile
echo 'export JAVA_HOME="/Applications/Android Studio.app/Contents/jre/jdk/Contents/Home/"' >> ~/.bash_profile
source ~/.bash_profile
```

Accept SDK licenses after installation and try a gradle command (e.g. `./gradlew build`) on a new project.

## Other useful software

Not required, but practical for debugging/development or if you would use Mac slave for web development. 

- Visual Studio Code: https://code.visualstudio.com/
  Add code to path https://code.visualstudio.com/docs/setup/mac#launching-from-the-command-line
- GitHub desktop: https://desktop.github.com/ 
- Google Chrome: https://www.google.com/chrome/ 
- Firefox: https://www.mozilla.org/hu/firefox/new/ 
- Docker: https://www.docker.com/ - download link after login
- Charles Proxy: https://www.charlesproxy.com/

## CLI

### zsh migration

Since Mac OS Catalina, `zsh` is the default shell instead of `bash`. This guide may contain references to `.bash_profile`, but you can either use `.zshrc` with the same content, or put the following lines to `.zshrc`:

```sh
if [ -f ~/.bash_profile ]; then 
    . ~/.bash_profile;
fi
```

#### node (with nvm)

Install node and npm with nvm. Link: https://github.com/nvm-sh/nvm

Install nvm:

```sh
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.38.0/install.sh | bash
```

Add to bash profile:

```sh
export NVM_DIR="$([ -z "${XDG_CONFIG_HOME-}" ] && printf %s "${HOME}/.nvm" || printf %s "${XDG_CONFIG_HOME}/nvm")"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh" # This loads nvm
```

Install node with nvm:

```sh
nvm install node 
```

Try node and npm:

```sh
node -v
npm -v
```

#### brew

Install brew (https://brew.sh/):

```sh
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Try and update brew:

```sh
brew doctor
brew update
```

#### jfrog

Install and try JFrog CLI (https://jfrog.com/getcli/):

```sh
npm install -g jfrog-cli-go
jfrog -v
```

#### ruby

With rbenv: https://github.com/rbenv/rbenv

Install & init rbenv:

```sh
brew install rbenv
rbenv init
```

Add `rbenv` init to `.bash_profile`:

```sh
echo 'eval "$(rbenv init -)"' >> ~/.bash_profile
```

Verify installation:

```sh
curl -fsSL https://github.com/rbenv/rbenv-installer/raw/master/bin/rbenv-doctor | bash
```

Select latest available ruby version:

```sh
rbenv install -l 
```

And install:

```sh
rbenv install <version-number>
```

Set installed ruby version global:

```sh
rbenv global <version-number>
```

And try (version output should be the same as the globally set):

```sh
ruby -v
```

If you have permission issues while installing gems, try following:

```sh
sudo chown -R $(whoami) ~/.rbenv
```

To link Rubies to Homebrew’s OpenSSL 1.1 (which is upgraded) add the following to `.zshrc` or `.bash_profile`:

```sh
export RUBY_CONFIGURE_OPTS="--with-openssl-dir=$(brew --prefix openssl@1.1)"
```

#### cocoapods

Install with gem:

```sh
sudo gem install cocoapods
```

Try cocoapods:

```sh
pod --version
```

#### fastlane

Install fastlane (https://docs.fastlane.tools/getting-started/ios/setup/): 

```sh
sudo gem install fastlane -NV
```

#### bundler

Install bundler (https://bundler.io/):

```sh
gem install bundler
```

If you have permission issues while using bundler, try following:

```sh
echo 'export BUNDLE_PATH=~/.gems' >> ~/.bash_profile
```

#### python

Install python with brew:

```sh
echo 'export PATH="/usr/local/opt/python/libexec/bin:$PATH"' >> ~/.bash_profile
brew install python
```

And try in new terminal window (version should be >= 3):

```sh
python --version
```

## Sonar-swift installation

Install sonar-swift (https://github.com/Backelite/sonar-swift)
Copy [run-sonar-swift.sh](https://raw.githubusercontent.com/Backelite/sonar-swift/master/sonar-swift-plugin/src/main/shell/run-sonar-swift.sh) somewhere to path, e.g. to `/usr/local/bin/run-sonar-swift`.

Install sonar-scanner:

```sh
npm install -g sonarqube-scanner
```

Install [xcpretty](https://github.com/supermarin/xcpretty) with following steps:

```sh
git clone https://github.com/Backelite/xcpretty.git
cd xcpretty
git checkout fix/duration_of_failed_tests_workaround
gem build xcpretty.gemspec
sudo gem install --both xcpretty-0.2.2.gem
```

Install [SwiftLint](https://github.com/realm/SwiftLint):

```sh
brew install swiftlint
```

Install [Tailor](https://github.com/sleekbyte/tailor):

```sh
brew cask install adoptopenjdk
brew install tailor
```

Install [slather](https://github.com/SlatherOrg/slather):

```sh
gem install slather
```

Install [lizard](https://github.com/terryyin/lizard):

```sh
sudo pip install lizard
```

Setup `sonar-project.properties` correctly within an iOS project and try:

```sh
run-sonar-swift
```

## Install Firebase CLI

```sh
curl -sL https://firebase.tools | bash
```

## Other useful CLI tools to setup, analyze and troubleshoot network connection

- wget - ```brew install wget```
- nmap - (https://nmap.org/download.html#macosx)
- telnet - ```brew install telnet```
- openconnect - ```brew install openconnect```
- tcpdump - installed by default

## Setup access for remote services

### SSH key

Create ssh key:

```sh
ssh-keygen -t rsa -b 4096
```

### Git

Configure git account...

**...for ssh connection:**

Copy your public key `pbcopy < ~/.ssh/id_rsa.pub` and add it to your account (GitHub, BitBucket etc.).

If your SSH key is secured with passphrase, the following steps are also necessary. Moreover, using an SSH key generated with non-empty passphrase for git, on a Jenkins slave, can cause connection issues.

Create `~/.ssh/config` to load passphrases into ssh-agent from keychain:

```
Host *
  AddKeysToAgent yes
  UseKeychain yes
  IdentityFile ~/.ssh/id_rsa
```

Add ssh key to agent:

```sh
ssh-add -K ~/.ssh/id_rsa
```

**...for HTTP connection:**

Enable git credential.helper to store passowrd

```sh
git config --global credential.helper store
```

and run `git pull` on a relevant repository to provide credentials.

### Articatory

Configure JFrog artifactory server in `jfrog` CLI and use it as default:

```sh
jfrog rt c rt-server-1 --insecure-tls=true --url=http://localhost:8081/artifactory --apikey=APIKEY
jfrog rt use rt-server-1
```

(type `n` twice for last command)

Try server connection:

```sh
jfrog rt ping --insecure-tls=true 
```

### Fastlane session authentication (to AppStore)

If you have 2-factory auth enabled in your AppStore account, you need to create a session every month and add it to `FASTLANE_SESSION` environment variable:

```sh
fastlane spaceauth -u user@email.com
```

Otherwise save credentials in keychain:

```sh
fastlane fastlane-credentials add --username user@email.com
```

### Firebase CLI authentication

Log in to your Firebase account from CLI:

```sh
firebase login
```

### Jenkins slave

Launch agent via...

**... SSH**

Get IP address of Mac OS and set up as node's host:

```sh
ipconfig getifaddr en0
```

Add jenkins user and password as credentials.

**... Java Web Start (launch agent by connecting it to the master)** 

Before using Java Web Start, make sure to configure following in Jenkins:

- Configure System -> Jenkins Location / Jenkins URL is set correctly (do not use localhost)
- Configure Global Security -> Enable security
- Configure Global Security -> Agents / TCP port for inbound agents	is set to "Fixed: 50000"

Install slave.jar:

```sh
curl -L --url https://YOUR_JENKINS_URL/jnlpJars/slave.jar --insecure -o /usr/local/lib/slave.jar 
```

Create LaunchAgents at `~/Library/LaunchAgents/org.jenkins-ci.slave.plist` with `UserName`, `jnlpUrl` and `-secret` properties set correctly:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-/Apple/DTD PLIST 1.0/EN" "http:/www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
    <key>Label</key>
    <string>jenkins</string>
    <key>ProgramArguments</key>
    <array>
        <string>java</string>
        <string>-jar</string>
        <string>/usr/local/lib/slave.jar</string>
        <string>-jnlpUrl</string>
        <string>http://YOUR_JENKINS_URL/computer/mac/slave-agent.jnlp</string>
        <string>-secret</string>
        <string>YOUR_SECRET</string>
    </array>
    <key>EnvironmentVariables</key>
    <dict>
        <key>PATH</key>
        <string>/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin</string>
    </dict>
    <key>KeepAlive</key>
    <true/>
    <key>RunAtLoad</key>
    <true/>
    <key>SessionCreate</key>
    <true/>
    <key>UserName</key>
    <string>YOUR_USERNAME</string>
    <key>WorkingDirectory</key>
    <string>/usr/local/var/jenkins</string>
    <key>StandardErrorPath</key>
    <string>/usr/local/var/jenkins/org.jenkins-ci.slave.log</string>
    <key>StandardOutPath</key>
    <string>/usr/local/var/jenkins/org.jenkins-ci.slave.log</string>
</dict>
</plist>
```

Load agent:

```sh
sudo launchctl load ~/Library/LaunchAgents/org.jenkins-ci.slave.plist
```

Give permission if required:

```sh
sudo chown root:wheel ~/Library/LaunchAgents/org.jenkins-ci.slave.plist
```

## Cleanup Xcode caches and archives periodically

Xcode archives and caches with continious builds can end up in large storage consupmtion. You can keep free disk space via scheduled cleanup.

Go to home dir:

```sh
cd ~
```

and create `cleanup.sh` file with following contents

```sh
#!/bin/bash

# clear unused devices
xcrun simctl delete unavailable

# clear archives older than 1 day
find ~/Library/Developer/Xcode/Archives -mtime +1 -delete

# clear simulator caches older than 1 day
find ~/Library/Developer/CoreSimulator/Caches/dyld/ -mtime +1 -delete
```

Schedule crontab to run `cleanup.sh` every day at 04:00.

```sh
crontab -e
```

Add following content:

```sh
0 4 * * * cd ~/ && ./cleanup.sh
```

Check crontab settings:

```sh
crontab -l
```

### Schedule a restart

It may worth to free up Mac OS system every now and then. For that schedule a restart at: `System Preferences` -> `Energy Saver` -> `Schedule...`

## Test slave

Follow instructions in [Mobile-CI Readme](https://github.com/Paldom/Mobile-CI/Readme.md)
