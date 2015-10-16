# Cheat Sheet - Apache - AWStats Installation

By Jack Szwergold, October 16, 2015

### Installing AWStats.

Go into the `/usr/share` directory:

    cd /usr/share

Grab a compressed archive of `awstats-7.3.tar.gz` from an official AWStats source site:

	sudo curl -O -L http://prdownloads.sourceforge.net/awstats/awstats-7.3.tar.gz

Next, decompress the archive like this:

	sudo tar -xf awstats-7.3.tar.gz

And delete the renmant `awstats-7.3.tar.gz` archive:
	
	sudo rm /usr/share/awstats-7.3.tar.gz

### Configuring Apache for AWStats.
	
Now let’s create our own `awstats.conf` like this:

	sudo nano /etc/apache2/conf.d/awstats.conf

Here is an example of a basic, non-secure Apache config for AWStats:

	Alias /awstatsclasses "/usr/share/awstats-7.3/wwwroot/classes/"
	Alias /awstatscss "/usr/share/awstats-7.3/wwwroot/css/"
	Alias /awstatsicons "/usr/share/awstats-7.3/wwwroot/icon/"
	Alias /icon "/usr/share/awstats-7.3/wwwroot/icon/"
	
	# The default method which doesn't allow directory indexing
	# ScriptAlias /awstats "/usr/share/awstats-7.3/wwwroot/cgi-bin"
	
	# Modified method that allows indexing
	Alias /awstats "/usr/share/awstats-7.3/wwwroot/cgi-bin"
	<Directory /usr/share/awstats-7.3/wwwroot/cgi-bin>
	  AddHandler cgi-script cgi pl
	  Options ExecCGI
	</Directory>

Here is an example of a basic, secure Apache config for AWStats. Note the `Allow from` exceptions; feel free to add any IP address you wish to bypass that secure setup to that list:

	Alias /awstatsclasses "/usr/share/awstats-7.3/wwwroot/classes/"
	Alias /awstatscss "/usr/share/awstats-7.3/wwwroot/css/"
	Alias /awstatsicons "/usr/share/awstats-7.3/wwwroot/icon/"
	Alias /icon "/usr/share/awstats-7.3/wwwroot/icon/"

	# The default method which doesn't allow directory indexing
	# ScriptAlias /awstats "/usr/share/awstats-7.3/wwwroot/cgi-bin"
	
	# Modified method that allows indexing
	Alias /awstats "/usr/share/awstats-7.3/wwwroot/cgi-bin"
	<Directory /usr/share/awstats-7.3/wwwroot/cgi-bin>
	  AddHandler cgi-script cgi pl
	  Options ExecCGI
	</Directory>
	
	<Directory "/usr/share/awstats-7.3/wwwroot">
	  Options FollowSymLinks
	  AllowOverride All
	
	  AuthName "AWStats Access"
	  AuthType Basic
	  require valid-user
	  AuthUserFile /etc/apache2/htpasswd_awstats
	
	  Order Deny,Allow
	  Deny from all
	  Allow from 127.0.0.1 ::1
	  Allow from localhost
	  Allow from 192.168
	  Allow from 10
	  Satisfy Any
	
	</Directory>

And if you are using the secure setup, be sure to setup the `/etc/apache2/htpasswd_awstats` that config refers to like this:

    sudo htpasswd -c /etc/apache2/htpasswd_awstats awstats

That command will initiate the process to create an `htpasswd` for a user named `awstats` in the file named `htpasswd_awstats`. When prompted, enter whatever password you would like to use.

### Installing `awstatstotals.php` as the index page for AWStats.

By default, AWStats has no formal `index.php` set which is a pain in the ass. But you can install `awstatstotals.php` as a nice index for the install like this.

First grab a copy of `awstatstotals.php` and place it in your user’s home directory. Then move it—and rename it to `index.php`—into the main AWStats code directory like this:

	sudo mv ~/awstatstotals.php /usr/share/awstats-7.3/wwwroot/cgi-bin/index.php

Then change the permissions of the file so it’s world readable:

	sudo chmod a+r /usr/share/awstats-7.3/wwwroot/cgi-bin/index.php

### Create the `data/` direcory.

Just create the `data/` direcory like this:

	sudo mkdir /usr/share/awstats-7.3/wwwroot/data

If you need to double-check anything in the core AWStats install, just check these directories:

	sudo ls -la /usr/share/awstats-7.3/wwwroot/data
	sudo ls -la /usr/share/awstats-7.3/wwwroot/cgi-bin

### Create a config for your web server.



    sudo cp /usr/share/awstats-7.3/wwwroot/cgi-bin/awstats.model.conf /usr/share/awstats-7.3/wwwroot/cgi-bin/awstats.www.example.com.conf

These 3 variables need to be set to whatever the logs are:

	sudo nano /usr/share/awstats-7.3/wwwroot/cgi-bin/awstats.www.example.com.conf
	
	LogFile="/var/log/apache2/www.example.com.access.log"
	SiteDomain="www.example.com"
	DirData="/usr/share/awstats-7.3/wwwroot/data"

***

	sudo chown root:root -R /usr/share/awstats-7.3
	
	sudo chmod g+w /usr/share/awstats-7.3/wwwroot/data

***

	*/30 * * * * /usr/share/awstats-7.3/wwwroot/cgi-bin/awstats.pl -config=www.example.com -update >/dev/null 2>&1
	
	/usr/share/awstats-7.3/wwwroot/cgi-bin/awstats.pl -config=www.example.com -update

#### File and directory ownership and perimssions for AWStats.

	sudo ls -la /etc/awstats
	sudo ls -la /usr/share/awstats
	sudo ls -la /usr/share/doc/awstats-6.95
	sudo ls -la /var/lib/awstats
	
	sudo chown awstats:awstats -R /etc/awstats
	sudo chown awstats:awstats -R /usr/share/awstats
	sudo chown awstats:awstats -R /usr/share/doc/awstats-6.95
	sudo chown awstats:awstats -R /var/lib/awstats
	
	sudo chmod 754 -R /etc/awstats
	sudo chmod 754 -R /etc/awstats/data
	sudo chmod 754 -R /usr/share/awstats
	sudo chmod 754 -R /usr/share/doc/awstats-6.95
	sudo chmod 754 -R /var/lib/awstats
	
	sudo chmod 755 /etc/awstats
	sudo chmod 755 /etc/awstats/data
	sudo chmod 755 /usr/share/awstats
	sudo chmod 755 /usr/share/doc/awstats-6.95
	sudo chmod 755 /var/lib/awstats
	
	sudo chmod 775 /usr/share/awstats/wwwroot/cgi-bin/

#### Known icons missing from AWStats.

	conf.png
	csv.png
	document.png
	dtd.png
	flv.png
	fon.png
	package.png
	runtime.png
	swf.png
	vbs.png
	xsl.png

#### URL with query stuff.

	# Keep or remove the query string to the URL in the statistics for individual
	# pages. This is primarily used to differentiate between the URLs of dynamic
	# pages. If set to 1, mypage.html?id=x and mypage.html?id=y are counted as two
	# different pages.
	# Warning, when set to 1, memory required to run AWStats is dramatically
	# increased if you have a lot of changing URLs (for example URLs with a random
	# id inside). Such web sites should not set this option to 1 or use seriously
	# the next parameter URLWithQueryWithOnlyFollowingParameters (or eventually
	# URLWithQueryWithoutFollowingParameters).
	# Change : Effective for new updates only
	# Possible values:
	# 0 - URLs are cleaned from the query string (ie: "/mypage.html")
	# 1 - Full URL with query string is used     (ie: "/mypage.html?p=x&q=y")
	# Default: 0
	#
	# URLWithQuery=0
	URLWithQuery=1

#### Config to report data on media files.

	# 2013-12-30: Adding extra section to track media.
	ExtraSectionName1="Hits on Images/Movies/Media"
	ExtraSectionCodeFilter1="200 304"
	ExtraSectionCondition1="URL,\.(png|gif|jpe?g|bmp|ico|mpg|mpeg|mp4|mov|flv|f4v|swf|mp3|aif?f|tif?f|xml)$"
	ExtraSectionFirstColumnTitle1="Image"
	ExtraSectionFirstColumnValues1="URL,^(\/.*\.(png|gif|jpe?g|bmp|ico|mpg|mpeg|mp4|mov|flv|f4v|swf|mp3|aif?f|tif?f|xml))$"
	ExtraSectionFirstColumnFormat1="<A HREF='http://cdn-home.guggenheim.org%s' TARGET='_blank'>%s</A>"
	ExtraSectionStatTypes1=HBL
	ExtraSectionAddSumRow1=1
	MaxNbOfExtra1=25
	MinHitExtra1=1

#### Config to report data on media file hotlinking.

	# 2013-12-30: Adding extra section to track hotlinking.
	ExtraSectionName2="Hotlinking pages"
	ExtraSectionCodeFilter2="200 304"
	ExtraSectionCondition2="URL,\.mpeg$||URL,\.mpg$||URL,\.avi$||URL,\.jpg$||URL,\.gif$||URL,\.png$||URL,\.bmp$||URL,\.jpeg||URL,\.mov||URL,\.flv||URL,\.f4v||URL,\.swf||URL,\.tif||URL,\.tiff||URL,\.xml$"
	ExtraSectionFirstColumnTitle2="Referrer"
	ExtraSectionFirstColumnValues2="REFERER,^(?!http:\/\/cdn-home.guggenheim.org)http:\/\/(.*)$"
	ExtraSectionFirstColumnFormat2="<a href='http://%s' target='_blank'>%s</a>"
	ExtraSectionStatTypes2=HBL
	ExtraSectionAddSumRow2=1
	MaxNbOfExtra2=25
	MinHitExtra2=1

***

*Cheat Sheet - Apache - AWStats Installation (c) by Jack Szwergold*

*This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC-BY-NC-SA-4.0).*