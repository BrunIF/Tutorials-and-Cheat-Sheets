# Cheat Sheet - Nmap

By Jack Szwergold, October 3, 2015

Scan a range via a wildcard and returns hostname as well as IP:

	nmap -sP 192.168.1.*

Scan a range via a slash notation and returns hostname as well as IP:

	nmap -sP 192.168.1.1/24

Scan a range via a slash notation and checks if ports `80` and `8080` are open and returns hostname as well as IP:

	sudo nmap -v -O 192.168.1.1/24 -p80,8080

Scans all ports on `192.168.1.1` in verbose mode using the `-v` flag:

	nmap -v 192.168.1.1

Scans all ports on `192.168.1.1` in verbose mode using the `-v` flag and attempts to detect OS via the `-O` flag:

    sudo nmap -v -O 192.168.1.1

***

*Cheat Sheet - Nmap (c) by Jack Szwergold*

*This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC-BY-NC-SA-4.0).*