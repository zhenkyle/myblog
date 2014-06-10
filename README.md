# myblog

This is my blog site's source.

Here is setup guide.

### Install rvm

```
\curl -sSL https://get.rvm.io | bash -s stable
source /home/zhenkyle/.rvm/scripts/rvm
rvm install ruby
rvm list
rvm use ruby
```

### Install git

```
[ubuntu] sudo apt-get install git
[osx] brew install git
```
### Install nodejs & npm

nodejs & npm is needed by foundation 5

```
[ubuntu] sudo apt-get install nodejs
[ubuntu] sudo apt-get install npm
[ubuntu] sudo ln -s /usr/bin/nodejs /usr/bin/node
[ubuntu] sudo npm install -g bower grunt-cli
[osx] brew install node
[osx] brew install npm 
[osx] npm install -g bower grunt-cli
```

### Install bundler

I use bundler to manager jekyll and foundation version

```
gem update --system
gem update
gem install bundler
```

### Git clone

```
git clone git@github.com:zhenkyle/myblog.git
```

### Setup requirments

```
cd myblog
npm install
bower install
bundle install --binstubs
bin/jekyll -v #2.0.3
bin/foundation version #1.0.4
```

### Start developing enviroment

build scss and watch for change

```
grunt
```

in another console, start jekyll serve and watch for change


```
jekyll serve -w
```


### Deloyment

I use git user Page to deploy, here are two simple rake tasks.

#### Setup deloy enviroment

```
rake setup
```

#### Deploy to git user page
```
rake deploy
```
