# Cheat Sheet - Mac OS X - Making a Bootable Mac OS X USB Flash Drive

By Jack Szwergold, October 6, 2015

### Creating a bootable Mac OS X USB flash drive in Mac OS X 10.7 and higher.

If you are using Mac OS X 1.7 (Lion) and higher, you can create bootable USB media using a command like this:

	sudo /path/to/Install\ OS\ X\ Yosemite.app/Contents/Resources/createinstallmedia --volume /Volumes/Untitled --applicationpath /path/to/Install\ OS\ X\ Yosemite.app --nointeraction

You need to set the path to the actual “Install OS X [Nickname].app” as well as the volume path to the USB flash drive. Just insert the USB flash drive, open up the Terminal and run that command. It will ask for your administrator password and then output would be something like this:

	Erasing Disk: 0%... 10%... 20%... 30%...100%...
	Copying installer files to disk...
	Copy complete.
	Making disk bootable...
	Copying boot files...
	Copy complete.
	Done.

The disk erasing process is pretty quick, but the “Copying installer files to disk...” takes forever. Just wait until that finishes and when it’s all done, it’s done and you will have bootable Mac OS X USB flash drive.

### An alternative way of creating a bootable Mac OS X USB flash drive in Mac OS X 10.6 and above.

The main benefit of this alternate method is it works in Mac OS X 10.6 (Snow Leopard) but the general procedure should work in any version of Mac OS X 10.6 and higher.

***

First open up the `InstallESD.dmg` hidden in the “Install OS X [Nickname].app” installer like this:

    open /path/to/Install\ OS\ X\ Yosemite.app/Contents/SharedSupport/InstallESD.dmg

Once that is mounted run `ls -la` on the mounted volume to see its contents:

    ls -la /Volumes/OS\ X\ Install\ ESD

Then run `asr` (Apple Software Restore) to restore the image to your USB flash drive. This command should work in Mac OS X 10.6:

    sudo asr restore --source /Volumes/OS\ X\ Install\ ESD/BaseSystem.dmg --target /Volumes/Untitled

This variant of the command should work in Mac OS X 10.7:

    sudo asr -restore -source /Volumes/OS\ X\ Install\ ESD/BaseSystem.dmg -target /Volumes/Untitled -erase -format HFS+ -noverify

The output would be something like this; when prompted to erase the USB flash drive respond `y`:

	Validating target...done
	Validating source...done
	Erase contents of /dev/disk3s2 (/Volumes/Untitled)? [ny]: y
	Retrieving scan information...done
	Validating sizes...done
	Restoring  ....10

The “Restoring” process will seemingly take forever but when it is done this will be the output:

    Restoring  ....10....20....30....40....50....60....70....80....90....100
	Remounting target volume...done

And that new volume would be mounted. But we’re not done yet! The issue is that image will have an alias to `Packages` but we need a full copy of the actual `Packages` directory to continue. And this is how we can do that.

First, let’s delete the `Packages` alias from the USB flash drive:

    rm /Volumes/OS\ X\ Base\ System/System/Installation/Packages

And now copy the actual `Packages` directory from `OS X Install ESD` to `OS X Base System/Installation/Packages`

    cp -rpv /Volumes/OS\ X\ Install\ ESD/Packages/ /Volumes/OS\ X\ Base\ System/System/Installation/Packages/

Now wait for that to copy to complete. It too will seem to take forever to copy, but when it is done the copy will simply stop. And you would thing we’d be done,  but one last thing.

You notice how the USB flash drive is generically named “OS X Base System?” Let’s change that to match the actual Mac OS installer being used on that USB flash drive like this:

    diskutil rename "OS X Base System" "Install OS X Yosemite"

Now, the bootable Mac OS X USB flash drive should be renamed ready for use.

***

*Cheat Sheet - Mac OS X - Making a Bootable Mac OS X USB Flash Drive (c) by Jack Szwergold*

*This work is licensed under a Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC-BY-NC-SA-4.0).*