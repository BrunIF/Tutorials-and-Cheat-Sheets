# Cheat Sheet - LS

By Jack Szwergold, September 17, 2015

***

List one item per line:

    ls -1

List items in a one line, comma separated list format:

    ls -m

List all of the items in the directory, order it by time and reverse the list:

    ls -latr

Do a directory search in reverse and find the last file with the `.sh` in the filname; can be changed to match anything else in the filename:

    ls -lrt | awk '/.sh/ { filename=$NF }; END { print filename }'

***

*Cheat Sheet - LS (c) by Jack Szwergold*

*This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC-BY-NC-SA-4.0).*