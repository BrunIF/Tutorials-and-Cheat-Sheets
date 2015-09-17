# Cheat Sheet - SSH

By Jack Szwergold, September 15, 2015

#### A fix for slow SSH client logins by disabling DNS lookups.

Sometimes slow SSH logins are a result of slow reverse DNS setups on a server. In the great scheme of things, DNS lookups for SSH logins is a fairly useless feature. So disabling DNS lookups is a potential fix to clear up slow SSH connections.

To attempt to fix this, open up the main `sshd_config` on the server:

    sudo nano /etc/ssh/sshd_config

Add this line to the bottom of the file:

    UseDNS no

After that’s done, if you are on Ubuntu/Debian, restart the `ssh` daemon like this:

    sudo service ssh restart

Or if you are on RedHat/CentOS, restart the `ssh` daemon like this:

    sudo service sshd restart

#### A fix for slow SSH client logins by changing the preferred order of authentication methods.

Sometimes slow SSH logins are caused by one item in the order of SSH authentication methods choking; making the SSH server “hang” while it waits to see if that choking authentication method ever responds. Chances are it won’t and will only wait until it times out and the next method can be used.

To attempt to fix this, open up the main `sshd_config` on the server:

    sudo nano /etc/ssh/ssh_config

Add this line to the bottom of the file:

    PreferredAuthentications publickey,password,gssapi-with-mic,hostbased,keyboard-interactive

Or add this on a per-user/per-client basis if access to a system-wide config on the server side is not practical:

    nano ~/.ssh/config

Add this line to the bottom of the file:

    Host *
	    PreferredAuthentications publickey,password

After that’s done, if you are on Ubuntu/Debian, restart the `ssh` daemon like this:

    sudo service ssh restart

Or if you are on RedHat/CentOS, restart the `ssh` daemon like this:

    sudo service sshd restart

#### A fix for `ssh_exchange_identification: Connection closed by remote host` errors.

If you have this issue, it might mean that `/var/empty/sshd` permissions are amiss. The permissions for `/var/empty/sshd` must be owned by root and not group or world-writable.

To attempt to fix this, login to the server that has this issue and run the following command:

    chmod go-w -R /var/empty

After that’s done, if you are on Ubuntu/Debian, restart the `ssh` daemon like this:

    sudo service ssh restart

Or if you are on RedHat/CentOS, restart the `ssh` daemon like this:

    sudo service sshd restart

#### A fix for `error: Could not load host key: /etc/ssh/ssh_host_ecdsa_key` errors.

If you have this issue, it might mean that there is a problem with the host key located in `/etc/ssh/ssh_host_ecdsa_key` and it should be regenerated.

To attempt to fix this, login to the server that has this issue and run the following command:

    sudo ssh-keygen -t ecdsa -f /etc/ssh/ssh_host_ecdsa_key -N ''

After that’s done, if you are on Ubuntu/Debian, restart the `ssh` daemon like this:

    sudo service ssh restart

Or if you are on RedHat/CentOS, restart the `ssh` daemon like this:

    sudo service sshd restart

#### Disable the SFTP subsystem in SSH.

Open up the main `sshd_config` on the server:

    sudo nano /etc/ssh/sshd_config

Find this line:

    Subsystem sftp /usr/lib/openssh/sftp-server

And comment it out:

    # Subsystem sftp /usr/lib/openssh/sftp-server

After that’s done, if you are on Ubuntu/Debian, restart the `ssh` daemon like this:

    sudo service ssh restart

Or if you are on RedHat/CentOS, restart the `ssh` daemon like this:

    sudo service sshd restart

***

*Cheat Sheet - SSH (c) by Jack Szwergold*

*This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC-BY-NC-SA-4.0).*