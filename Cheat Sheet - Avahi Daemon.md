# Cheat Sheet - Avahi Daemon

By Jack Szwergold, September 15, 2015

Avahi daemon is a zero-configuration (zeroconf)/multicast networking implementation that is similar to the “Bonjour” service in Mac OS X. Very useful in getting a Linux box to broadcast it’s services across a network.

#### Installation and configuration.

Install the Avahi daemon.

    sudo aptitude install avahi-daemon avahi-utils

Edit the Avahi daemon config.

    sudo nano /etc/avahi/avahi-daemon.conf

#### Some example Avahi service configs.

An example AFP (Apple Filing Protocol) service (`afpd.service`):

	sudo nano /etc/avahi/services/afpd.service
	
	<?xml version="1.0" standalone='no'?><!--*-nxml-*-->
	<!DOCTYPE service-group SYSTEM "avahi-service.dtd">
	<service-group>
	  <name replace-wildcards="yes">%h AFP</name>
	  <service>
	    <type>_afpovertcp._tcp</type>
	    <port>548</port>
	  </service>
	</service-group>

An example Apache web server service (`apache.service`):

	sudo nano /etc/avahi/services/apache.service
	
	<?xml version="1.0" standalone='no'?><!--*-nxml-*-->
	<!DOCTYPE service-group SYSTEM "avahi-service.dtd">
	<service-group>
	  <name replace-wildcards="yes">%h HTTP</name>
	  <service>
	    <type>_http._tcp</type>
	    <port>80</port>
	  </service>
	</service-group>


An example VNC (Virtual Network Computing) service (`rfb.service`):

	sudo nano /etc/avahi/services/rfb.service
	
	<?xml version="1.0" standalone='no'?><!--*-nxml-*-->
	<!DOCTYPE service-group SYSTEM "avahi-service.dtd">
	<service-group>
	  <name replace-wildcards="yes">%h VNC</name>
	  <service>
	    <type>_rfb._tcp</type>
	    <port>5901</port>
	  </service>
	</service-group>

An example SMB (Samba) service (`samba.service`):

	sudo nano /etc/avahi/services/samba.service
	
	<?xml version="1.0" standalone='no'?><!--*-nxml-*-->
	<!DOCTYPE service-group SYSTEM "avahi-service.dtd">
	<service-group>
	  <name replace-wildcards="yes">%h</name>
	  <service>
	    <type>_smb._tcp</type>
	    <port>139</port>
	  </service>
	</service-group>

An example SFTP (Secure FTP) service (`sftp.service`):

	sudo nano /etc/avahi/services/sftp.service
	
	<?xml version="1.0" standalone='no'?><!--*-nxml-*-->
	<!DOCTYPE service-group SYSTEM "avahi-service.dtd">
	<service-group>
	  <name replace-wildcards="yes">%h SFTP</name>
	  <service>
	    <type>_sftp-ssh._tcp</type>
	    <port>22</port>
	  </service>
	</service-group>

An example SSH (Secure Shell) service (`ssh.service`):

	sudo nano /etc/avahi/services/ssh.service
	
	<?xml version="1.0" standalone='no'?><!--*-nxml-*-->
	<!DOCTYPE service-group SYSTEM "avahi-service.dtd">
	<service-group>
	  <name replace-wildcards="yes">%h SSH</name>
	  <service>
	    <type>_ssh._tcp</type>
	    <port>22</port>
	  </service>
	</service-group>

***

*Cheat Sheet - Avahi Daemon (c) by Jack Szwergold*

*This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC-BY-NC-SA-4.0).*