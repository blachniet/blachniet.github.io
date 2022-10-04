---
title: "746GB in a 3TB Seagate Drive"
date: "2013-06-02"
categories:
- "site-updates"
tags:
- "hardware"
aliases:
- "/blog/746gb-in-a-3tb-seagate-drive/"
blachnietWordPressExport: true
---

So I finally got the time to install my 3TB hard drive in my home server, but ran into some problems. After installing the drive, it showed only 746GB available on the drive. I know I'm never going to see 3000GB (because [hard drive manufacturers are liars](http://www.navarinounincorporated.com/rants/index.php/2010/02/15/hard-drive-and-monitor-manufacturers-lie)) but this was way below the expected size.

Luckily, [I'm not the only one that's run into this issue](http://www.tomshardware.com/forum/287288-32-seagate-hard-drive-showing-746gb). My first instict was to go to [Disk Management](http://lmgtfy.com/?q=how+to+get+to+disk+management) to try to extend the volume, but when I got in there, it only showed the single volume with 746GB, and no unallocated space. To resolve this, I did the following:

1. Downloaded and installed [Segate's DiscWizard](http://www.seagate.com/support/internal-hard-drives/enterprise-hard-drives/savvio-15k/discwizard-master-dl/)
2. Opened the **DiscWizard** - Just ran it, didn't do anything in it.
3. Closed and reopened [Disk Management](http://lmgtfy.com/?q=how+to+get+to+disk+management)
4. My drive now showed unallocated space, so I extended the existing volume to include the rest of the space.

I'm not really sure why this works, but it did. If you run into issues, try using the **DiscWizard** to try to extend the volume.
