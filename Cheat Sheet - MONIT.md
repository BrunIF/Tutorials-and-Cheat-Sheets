# Cheat Sheet - MONIT

By Jack Szwergold, September 15, 2015

#### Getting `monit` installed and other basics.

First, install `monit` via `aptitude` like this:

    sudo aptitude install monit

Controls `monit` startup; don’t need to adjust for now:

    sudo nano /etc/default/monit

Main config for `monit`; broad adjustments happen here:

    sudo nano /etc/monit/monitrc

Adjust the default daemon values to check services every 60 seconds with a start delay of 120:

	set daemon 60
	with start delay 120

Then find the `mailserver` area of `monitrc` and add the following line. Postfix or an SMTP needs to be active for this to work:

    set mailserver localhost

Create the `conf.d` monit directory if it doesn’t exist:

	sudo mkdir -p /etc/monit/conf.d

Follow the `monit` log to see it in action:

    sudo tail -f -n 200 /var/log/monit.log

#### Create a custom `monit` Apache status monitoring rule.

First, check to see if the `apache2.pid` file exists:

    ls -la /var/run/apache2.pid

Now create the actual Apache monitoring rule for `monit`:

    sudo nano /etc/monit/conf.d/apache2.conf

One type of Apache monitoring rule:

	check process apache with pidfile /var/run/apache2.pid
      start "/etc/init.d/apache2 start"
      stop  "/etc/init.d/apache2 stop"
      if failed host 127.0.0.1 port 80
        with timeout 15 seconds
      then restart
      alert email_address@example.com only on { timeout, nonexist }

Another type of Apache monitoring rule:

	check process apache with pidfile /var/run/apache2.pid
      start "/etc/init.d/apache2 start"
      stop  "/etc/init.d/apache2 stop"
      if failed host 127.0.0.1 port 80
        with timeout 15 seconds
      then restart
      if loadavg (1min) greater than 7
        for 5 cycles
      then restart
      alert email_address@example.com only on { timeout, nonexist, resource }
	
Restart `monit` and all should be good:

    sudo service monit restart

#### Create a custom `monit` MySQL status monitoring rule.

First, check to see if the `apache2.pid` file exists:

    ls -la /var/run/mysqld/mysqld.pid

Now create the actual MySQL monitoring rule for `monit`:

    sudo nano /etc/monit/conf.d/mysql.conf

One type of MySQL monitoring rule:

	check process mysqld with pidfile /var/run/mysqld/mysqld.pid
	  group mysql
	  start program = "/etc/init.d/mysql start"
	  stop program = "/etc/init.d/mysql stop"
	  if failed host 127.0.0.1 port 3306
	    with timeout 15 seconds
	  then restart
	  if 5 restarts within 5 cycles
	  then timeout
	  alert email_address@example.com only on { timeout, nonexist }

Restart `monit` and all should be good:

    sudo service monit restart

***

*Cheat Sheet - MONIT (c) by Jack Szwergold*

*This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC-BY-NC-SA-4.0).*