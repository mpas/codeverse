+++
title 		= "Automount NTFS volume under Linux (Mint)"
tags 		= ["linux", "mint"]
categories	= ["Operating Systems"]
date		= "2013-05-28"
+++

When automounting an NTFS volume under Linux (Mint) you can do this using the /etc/fstab file.

	$ sudo pico /etc/fstab

Add a line in the /etc/fstab file:

	# custom mount point
	/dev/sdb1 /media/windows-c ntfs-3g defaults 0 0

And take the mount into effect.

	$ mount -a