<blockquote>
<b>Tested on:</b><br />
Macbook Pro (with Retina display)<br />
Specs: 2.4GHz Intel Core i5, 8 GB 1600 MHz DDR3, Intel Iris Graphics<br />
OS: Mac OSX Mavericks 10.9.4 (13E28)<br />
</blockquote>

<center><img src="http://imgs.xkcd.com/comics/im_an_idiot.png"></center>

### Update and install Mac stuff
1. Install MacOSX updates from App Store. 
2. Git comes pre-bundled with Xcode. Also, loads of other development softwares depend on Xcode. So, install [Xcode from App Store](https://itunes.apple.com/en/app/xcode/id497799835?mt=12). Mind you, Xcode is a big download, so be prepared to wait for some time. Go and grab a <strike>beer</strike> coffee.
3. After Xcode is installed, open Xcode once and agree to license agreement (some programs we install later in this article are picky about the user agreeing to Xcode license agreement; so, it's better to agree to it in advance). After you have opened and agreed to Xcode's license agreement, you can close Xcode.
3. To install **Xcode Command Line Developer Tools**, run this command:
<pre>
xcode-select --install
</pre>
You will now see a pop-up window prompting you to install, after clicking "Install" button, just sit back and wait for it to finish.
4. **Optional-ish**: To setup `git` (installed in previous step), follow [official documentation](https://help.github.com/articles/set-up-git#platform-mac).
4. **Optional**: If you need a GUI for `git`, download and install [GitHub for Mac](https://mac.github.com/). Installation is pretty straightforward (point-n-click) and [click here for First Launch](https://help.github.com/articles/first-launch) documentation and here are [more docs](https://help.github.com/categories/31/articles).

### Setup Homebrew
To install [Homebrew](http://brew.sh/), paste this and run on Terminal.app prompt:
<pre>ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"</pre>
The script explains what it will do and then pauses before it does it.

After you have installed Homebrew, run:
<pre>brew doctor</pre>
If everything is ok, this command will output:
<blockquote>Your system is ready to brew.</blockquote>
Update Homebrew's package database:
<pre>brew update</pre>
For more details, you can read the official documentation [here](https://github.com/Homebrew/homebrew/wiki/Installation).

### Setup Homebrew Cask
Install [Homebrew cask](http://caskroom.io/) using:
<pre>brew install caskroom/cask/brew-cask</pre>
**Optional but recommended**: Enable alternate versions of Casks:
<pre>brew tap caskroom/versions</pre>

### Install nifty tools
<pre>
# Terminal multiplexer (like 'screen', which is installed on OSX by default):
brew install tmux

# Network downloader
brew install wget

# ImageMagick
brew install imagemagick

# Python 2.7
brew install python

# AWS CLI
pip install awscli
</pre>

### Install Ruby and Rails (using `rvm`)
<pre>
# Install 'rvm'
\curl -sSL https://get.rvm.io | bash -s stable
source ~/.rvm/scripts/rvm

# Install Ruby 1.9.3 and set it as default Ruby version system-wide
rvm use --install 2.1.5
rvm use 2.1.5
rvm alias create default 2.1.5

# Install Rails 3.2.11
gem install rails -v 4.2.1
</pre>

### Setup MongoDB
Now let's [install MongoDB using HomeBrew](http://docs.mongodb.org/manual/tutorial/install-mongodb-on-os-x/#install-mongodb-with-homebrew): 
<pre>
# Install MongoDB:
brew install mongodb

# To have launchd start mongodb at login:
ln -sfv /usr/local/opt/mongodb/*.plist ~/Library/LaunchAgents

# To load mongodb right now:
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.mongodb.plist
</pre>

### Install and setup PostgreSQL and PostGIS
#### Install PostgreSQL and PostGIS
[Postgres.app](http://postgresapp.com/) is the easiest way to get started with PostgreSQL on the Mac. Open the app, and you have a PostgreSQL server ready and awaiting new connections. Close the app, and the server shuts down.

Whether you're a command line aficionado, prefer GUIs, or just want to start making things with your framework of choice, connecting to Postgres.app is easy.

Download graphical/GUI installer for Postgres.app from [this link](https://github.com/PostgresApp/PostgresApp/releases/download/9.4.2.0/Postgres-9.4.2.0.zip). Just download, unzip the downloaded file, drag `Postgres.app` icon to the `Applications\` folder, and double-click to start PostgreSQL. This will install:
<blockquote>
<ul>
<li>PostgreSQL 9.4.2</li>
<li>PostGIS 2.1.7</li>
<li>Procedural languages: PL/pgSQL, PL/Perl, PL/Python, and PLV8 (Javascript)</li>
<li>A number of command-line utilities for managing PostgreSQL and working with GIS data</li>
</ul>
</blockquote>
To use [commands that are installed with Postgres.app](http://postgresapp.com/documentation/cli-tools.html) (especially `psql`, `createuser`, `pg_dump` etc), run:

<pre>
echo 'export PATH=$PATH:/Applications/Postgres.app/Contents/Versions/9.4/bin' >> ~/.bash_profile
source ~/.bash_profile 
</pre>

#### Install `pg` gem
Now [install the `pg` gem](http://postgresapp.com/documentation/configuration-ruby.html) by executing the following command:
<pre>
sudo ARCHFLAGS="-arch x86_64" gem install pg
</pre>
Then according to [this documentation](), in `config/database.yml`, use the following settings:
<pre>
development:
  adapter: postgresql
  database: <b><i>[YOUR_DATABASE_NAME]</i></b>
  host: localhost
</pre>

#### Create PostgreSQL users
Let's create the PostgreSQL users for our Rails app:

<pre>createuser --pwprompt --superuser pgdev-admin</pre>

Enter `test123` as password.

<pre>createuser --pwprompt pgdev</pre>

Enter `test123` as password again.

#### Enabling PostGIS

PostGIS was installed alongwith [Postgres.app](http://postgresapp.com/). Installing PostGIS is just the first step. PostGIS is an optional extension that must be enabled in each database you want to use it in before you can use it.

Now, to [enable PostGIS](http://postgis.net/install), type `psql` on Terminal.app. Run the following SQL on psql prompt/console:
<pre>
-- Enable PostGIS (includes raster)
CREATE EXTENSION postgis;

-- Enable Topology
CREATE EXTENSION postgis_topology;

-- fuzzy matching needed for Tiger
CREATE EXTENSION fuzzystrmatch;

-- Enable US Tiger Geocoder
CREATE EXTENSION postgis_tiger_geocoder;
</pre>
Now press `\q` to exit `psql` prompt.

### Install Redis
Run:
<pre>
# Install Redis:
brew install redis

# To have launchd start redis at login:
ln -sfv /usr/local/opt/redis/*.plist ~/Library/LaunchAgents

#Then to load redis now:
launchctl load ~/Library/LaunchAgents/homebrew.mxcl.redis.plist
</pre>

### Install LaunchRocket
<pre>
# A Mac PrefPane to manage all your Homebrew-installed services
# https://github.com/jimbojsb/launchrocket
brew cask install launchrocket
</pre>
Learn what LaunchRocket is and how to use it [here](https://github.com/jimbojsb/launchrocket).

### Other useful commands [OPTIONAL]
You don't necessarily need to run these commands. If you don't understand what these commands mean, then either [JFGI](http://justfuckinggoogleit.com/) or ignore them. These are **optional** commands for the curious developers:

1. If you don't want/need launchctl for MongoDB, you can just run: <pre>mongod --config /usr/local/etc/mongod.conf</pre>

2. If you don't want/need launchctl for Redis, you can just run: <pre>redis-server /usr/local/etc/redis.conf</pre>

### Basic Utility Tools
Git, Sublime, Android Studio are available as .dmg package. Here are the steps to follow to install any package from .dmg file<br>
http://www.ofzenandcomputing.com/how-to-install-dmg-files-mac/

1. For Android Java is Pre-Requirement Go To : http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html & then install jdk using dmg file<br>
2. Download Android Studio from Google Android Studio Official Website - https://developer.android.com/sdk/installing/index.html?pkg=studio
