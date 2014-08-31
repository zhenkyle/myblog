---
layout: post
title:  "Install java development enviroment on OSX Mavericks"
date:   2014-08-31 10:34:00
categories: java
---

### Install jdk 8u20 by hand
Mac OSX 10.6 and before shipped with Java 6, OSX 10.7 and above doesn't shiped with Java.When execute `java -version`
command in Mavericks Terminal, a window popup, inform you that JDK is not installed , click on the "more information" 
button, will take you to the Oarcle site to download the most current jdk package. The most current jdk version is `8u20`.

[Reference Orcale java mac FAQ](http://www.java.com/en/download/faq/java_mac.xml)
[Reference](http://osxdaily.com/2013/11/01/install-java-os-x-mavericks/)

### Install jdk 6 from Apple
Mavericks doesn't shipped with jdk 6. But jdk 6 can be obtained from Apple's site. The most current jdk 6 version 
from Apple is `Java for OS X 2014-001` .Here is Download link.

[Java 6 for OSX by Apple](http://support.apple.com/kb/DL1572)

### Recommend way to install jdk and other develop tools
The recommend way to install jdk and other develop tools is by [home brew](http://brew.sh/).
We like package management.

First, install Homebrew.
```
ruby -e "$(curl -fsSL https://raw.github.com/Homebrew/homebrew/go/install)"
```

Second, install [Homebrew Cask]http://caskroom.io/)
```
brew install caskroom/cask/brew-cask
```

Then,  install java with the most current version (8u20 at this time)
```
brew cask install java
```

### Install other things
```
brew install ant
brew install maven
```
[Reference](http://derjan.io/blog/2013/11/25/setup-mac-for-development/)
[Reference](http://ksmx.me/homebrew-cask-cli-workflow-to-install-mac-applications/)
[Reference](http://www.yangzhiping.com/tech/homebrew-cask.html)
[Reference](https://www.google.com.hk/search?q=home+brew+cask)

### If need to install multiple java version
[Reference](http://hanxue-it.blogspot.com/2014/05/installing-java-8-managing-multiple.html)
