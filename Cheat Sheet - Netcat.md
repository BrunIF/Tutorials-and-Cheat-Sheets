# Cheat Sheet - Netcat

By Jack Szwergold, October 17, 2015

#### Create a simple one-line web server with Netcat.

First, create a simple `index.html` file like this:

    echo Hello world\! > index.html

Now create the web server with Netcat like this; it will be available on `localhost` at port `8000`:

    while true; do { echo -e "HTTP/1.1 200 OK\r\n"; cat index.html; } | nc -l 8000; done

Then just check the URL in a web browser like this:

    localhost:8000

And you should see the contents of `index.html` (aka: “Hello world!”) show up in your browser.

***

*Cheat Sheet - Netcat (c) by Jack Szwergold*

*This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC-BY-NC-SA-4.0).*
