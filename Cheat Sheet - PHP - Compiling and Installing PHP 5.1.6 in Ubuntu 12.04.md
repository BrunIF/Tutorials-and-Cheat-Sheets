# Cheat Sheet - PHP - Compiling and Installing PHP 5.1.6 in Ubuntu 12.04

By Jack Szwergold, October 13, 2015


### Backup the already installed Apache PHP module. 

Before you do anything else, create a backup of the already installed `libphp5.so` module by copying it like this:

	sudo cp /usr/lib/apache2/modules/libphp5.so /usr/lib/apache2/modules/libphp5310.so

The name `libphp5310.so` is derived from the PHP 5.3.10 version of `libphp5.so` that is currently available on Ubuntu 12.04. Adjust name as needed but be sure to rename `libphp5.so` before compiling and installing so you are not left flying in the wind of this setup goes bad.

### Install the PHP source code prerequisites and dependencies.

First, run `aptitude update` like this:

    sudo aptitude update

Install the basics for the build:

    sudo aptitude install build-essential debhelper fakeroot \
                          apache2 php5-gd php-pear libpcre3-dev bzip2

Now install APC via Pecl like this:

    sudo pecl install -f apc




Now install all of the development libraries and other stuff:

	sudo aptitude install apache2-threaded-dev \
	                      libjpeg8 libjpeg8-dev libxml2-dev \
	                      libz-dev libfreetype6-dev libpng12-0-dev \
	                      libbz2-dev libgmp3-dev libpspell-dev \
	                      php-gettext gettext \
	                      mcrypt libmcrypt-dev \
	                      pkg-config libcurl4-openssl-dev \
	                      flex libfl-dev \
                          libmysqld-dev

#### Set symbolic links for libraries.

Set the PCRE lib symbolic link for Ubuntu:

    sudo ln -s /usr/lib/x86_64-linux-gnu/libpcre.so /usr/lib/libpcre.so

Set the JPEG lib symbolic link for Ubuntu:

    sudo ln -s /usr/lib/x86_64-linux-gnu/libjpeg.so /usr/lib/libjpeg.so

Set the PNG lib symbolic link for Ubuntu:

    sudo ln -s /usr/lib/x86_64-linux-gnu/libpng.so /usr/lib/libpng.so

Set the `libmysqlclient` symbolic link for Ubuntu 12.04:

	sudo ln -s /usr/lib/x86_64-linux-gnu/libmysqlclient.so /usr/lib/libmysqlclient.so

#### Get the PHP source code.

Download the source code:

	curl -O -L http://museum.php.net/php5/php-5.1.6.tar.gz

Decompress the PHP source code:

	tar -xf php-5.1.6.tar.gz

#### Get the PHP Suhosin patch.

Get the PHP Suhosin patch:

	curl -O -L http://download.suhosin.org/suhosin-patch-5.1.6-0.9.6.patch.gz

Decompress the Suhosin patch:

	gzip -d suhosin-patch-5.1.6-0.9.6.patch.gz

Go into the source directory:

	cd ~/php-5.1.6

Apply the Suhosin patch:

	patch -p 1 -i ~/suhosin-patch-5.1.6-0.9.6.patch

Create the install destination directory:

	sudo mkdir -p /opt/php516-gd

Configure, make and install:

	./configure --prefix=/opt/php516-gd --with-iconv --with-jpeg-dir=/usr --with-png-dir=/usr --with-zlib-dir=/usr \
	    --with-freetype-dir=/usr --enable-gd-native-ttf --with-gd --with-apxs2=/usr/bin/apxs2 \
		--with-pic --with-bz2  --with-curl --with-gettext --with-gmp --with-openssl --with-pspell \
		--with-expat-dir=/usr --with-zlib --enable-exif --enable-ftp --enable-mbstring --with-gettext \
		--enable-magic-quotes --enable-sockets -enable-sysvsem --enable-sysvshm --enable-sysvmsg \
		--enable-track-vars --enable-trans-sid --enable-wddx --with-kerberos --enable-memory-limit \
		--enable-shmop --enable-calendar --enable-dbx --enable-dio --with-xml --enable-apc --enable-apc-mmap \
		--with-pcre-regex=/usr --enable-utf8 \
		--with-mysql=/usr --with-pdo-mysql=/usr --with-mcrypt=/usr

Run `make` and `make install`:

	make
	sudo make install

Run `nohup make` and `make test`:

	nohup make > ~/make_php-5.1.6.txt &
	
Follow the output of the `nohup make` and `make test`:

	tail -f ~/make_php-5.1.6.txt

After the module is installed, rename it to match the version/patch:

	sudo mv /usr/lib/apache2/modules/libphp5.so /usr/lib/apache2/modules/libphp516-gd.so

The edit the Apache2 PHP config to allow for easy switching between different versions.

	sudo nano /etc/apache2/mods-available/php5.load

An example of what should be in the Apache2 PHP config:

	# LoadModule php5_module        /usr/lib/apache2/modules/libphp5310.so
	LoadModule php5_module        /usr/lib/apache2/modules/libphp516-gd.so

Set the `php.ini` file for PHP 5.1.6:

	sudo cp ~/php-5.1.6/php.ini-dist /opt/php516-gd/lib/php.ini

Edit the `php.ini` file for PHP 5.1.6:

	sudo nano /opt/php516-gd/lib/php.ini

### How to turn off FreeType hinting in PHP GD 2.0 and lower.

If you’re interested in turning off FreeType hinting, search for the following line in the GD source (gdft.c):

    err = FT_Load_Glyph (face, glyph_index, FT_LOAD_DEFAULT);

and replace it with

    err = FT_Load_Glyph (face, glyph_index, FT_LOAD_NO_HINTING);

How to turn off FreeType hinting in PHP GD 2.0.13:

	sudo nano [path to php source files] ext/gd/libgd/gdft.c
	
	sudo nano ~/php-5.1.6/ext/gd/libgd/gdft.c

Search for `render_mode`; should look like this: 

	int render_mode = FT_LOAD_DEFAULT;
	int render_mode = FT_LOAD_RENDER;

Change to:

	int render_mode = FT_LOAD_NO_HINTING;

***

*Cheat Sheet - PHP - Compiling and Installing PHP 5.1.6 in Ubuntu 12.04 (c) by Jack Szwergold*

*This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC-BY-NC-SA-4.0).*
