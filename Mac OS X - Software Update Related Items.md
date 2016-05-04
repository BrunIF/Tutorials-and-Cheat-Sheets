## Mac OS X - Software Update Related Items

By Jack Szwergold, October 6, 2015

Setting the software updates server from the command line:

	sudo defaults write /Library/Preferences/com.apple.SoftwareUpdate CatalogURL "http://example.local:8088/index.sucatalog"

Shows the software updates settings:

	defaults read /Library/Preferences/com.apple.SoftwareUpdate

Resets the catalog URL to point to Apple:

	sudo defaults delete /Library/Preferences/com.apple.SoftwareUpdate CatalogURL

***

<sup>*Mac OS X - Software Update Related Items (c) by Jack Szwergold. This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC-BY-NC-SA-4.0).*</sup>