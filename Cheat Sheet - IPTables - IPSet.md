# Cheat Sheet - IPTables - IPSet

By Jack Szwergold, October 27, 2015

#### Installing IPSet.

Install `ipset`:

    sudo aptitude install ipset

#### Some of the basics.

Create an IP set like this:

    sudo ipset -N FooBar hash:net

Check that the IP set exists by listing the all sets like this:

	sudo ipset -l

The output at this point would be something like this:

	Name: FooBar
	Type: hash:net
	Header: family inet hashsize 1024 maxelem 65536
	Size in memory: 16760
	References: 0
	Members:

Now let’s add a few IPs to that set like this:

    sudo ipset add FooBar 10.0.0.0/8
    sudo ipset add FooBar 127.0.0.0/8
    sudo ipset add FooBar 192.168.0.0/16

And if you run IP set again:

	sudo ipset -l

The additions will be reflected in the new output:

	Name: FooBar
	Type: hash:net
	Header: family inet hashsize 1024 maxelem 65536
	Size in memory: 16856
	References: 0
	Members:
	127.0.0.0/8
	192.168.0.0/16
	10.0.0.0/8

And can then save the IP set like this

    sudo ipset save FooBar > ipset.FooBar.conf

The contents of `ipset.FooBar.conf` it will look something like this:

	create FooBar hash:net family inet hashsize 1024 maxelem 65536
	add FooBar 127.0.0.0/8
	add FooBar 10.0.0.0/8
	add FooBar 192.168.0.0/16

At this point if you wanted to destroy the `FooBar` IP set, you can do so by doing this:

    sudo ipset destroy FooBar

And you can restore the IP set like this:

    sudo ipset restore < ipset.FooBar.conf

#### How to block a whole country’s IP range with IPSet.

Create a China specific (`CN`) IP set like this:

	sudo ipset -N CN_range hash:net

Check that the IP set exists by listing the all sets like this:

	sudo ipset -l

The output at this point would be something like this:

	Name: CN_range
	Type: hash:net
	Header: family inet hashsize 1024 maxelem 65536
	Size in memory: 16760
	References: 0
	Members:

Now let’s download the raw `cn.zone` file from the IP Deny website like this:

	curl -O -L http://www.ipdeny.com/ipblocks/data/countries/cn.zone

And now let’s populate the `CN_range` IP set with the values from the `cn.zone` file:

	awk '{print "add CN_range " $0}' cn.zone >> ipset.CN_range.conf

And finally let’s tell IPTables to pay attention to that `CN_range` set like this:

	sudo iptables -A INPUT -p tcp -m set --match-set CN_range src -j DROP

***

*Cheat Sheet - IPTables - IPSet (c) by Jack Szwergold*

*This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC-BY-NC-SA-4.0).*
