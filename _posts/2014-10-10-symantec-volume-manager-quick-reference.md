---
layout: post
title: "My Symantec Volume Manager Quick Reference"
description: ""
category: 
tags: ["veritas"]
---
{% include JB/setup %}

My old Veritas Volume Manager quick reference from back in the day!  Note that it's Solaris-centric but almost all of the information can be used on Linux too.


## Misc Information

* Sizes can be specified with the b, k, m and g modifiers which are blocks, kilobytes, megabytes and gigabytes respectively.  For example, 3g is 3 gigabytes and 700m is 700 megabytes.  No letter is specified the size is assumed to be 512 byte blocks/sectors
* All commands can be used with the -g disk-group option and argument to specify a disk group other than the default (rootdg)
* **Disk Media Name** is the administrative VxVM name like disk01
* **Disk Access Name** is the device name such as c0t0d0
* Some commands are in ``/etc/vx/bin``
* Remember, if the OS can't see the disks the VxVM can't see them, use ``format`` to ensure the OS can see the disks


## Displaying Information

* ``vxprint -ht`` - The most commonly used command to display information
* ``vxprint -rht`` - Same as above but displays layered volume information as well
* ``vxprint -A`` - Displays all information about all imported disk groups and associated disks as well as volumes and their associated plexes and subdisks
* ``vxprint -G`` - Displays information about all disk groups
* ``vxprint -v`` - Displays volumes, FS type and size in 512 byte blocks for each
* ``vxprint -p`` - Displays plexes, the volume each is associated with and the size of the volume in 512 byte blocks
* ``vxprint -s`` - Displays subdisks, the plex each is associated with and the size of the volume in 512 byte blocks
* ``vxprint -d`` - Displays disks and the physical disk each is associated with (c0t0d0s2 for example)
* ``vxprint -S`` - Displays summary information including the total number of disks, subdisks, plexes and volumes, unused subdisks and unused slexes in each disk group
* ``vxprint -l another-option-here`` - Prints verbose information for the each record (``vxprint -v -l`` for example)
* ``vxprint -l object`` - Prints verbose information for *object* such as a subdisk or plex
* ``vxprint -r options`` - Displays information for layered volumes, *options* are optional

* ``vxinfo`` - Displays the status of all volumes (started, unstartable, startable and started unusable)
* ``vxinfo -p`` - Same as above but lists information for each plex as well

* ``vxdg free`` - Displays the amount of free space on each disk in the disk group, in sectors


## Volume Operations

Volumes must be started before they can be mounted and fsck'ed!

* ``vxvol start volume`` - Starts *volume* and performs recovery operations as necessary
* ``vxvol startall`` - Starts all volumes in the default disk group (rootdg)
* ``vxvol stop volume`` - Stops *volume*. Note that you must stop any applications and unmount the file system using the Volume before stopping it
* ``vxvol stopall`` - Stops all volumes in the default disk group (rootdg). Note that you must stop all applications and unmount the file systems using the Volumes before stopping them

* ``vxmake -U fsgen vol volume plex=plex01,plex02,...`` - Creates a new volume named *volume* that consists of the specified plexes

* ``vxedit -r rm volume`` - Deletes *volume* and all of its associated objects (plexes and subdisks), make sure the volume is unmounted and stopped before you do this
* ``vxedit rm volume`` - Deletes volume, it must have no associated plexes
* ``vxedit rename old-name new-name`` - Renames a volume from *old-name* to *new-name*

* ``vxr5check volume`` - Verifies the parity of the RAID 5 of *volume*, do not use this command if the volume is in degraded mode

* ``vxassist make volume size`` - Creates the new Volume named volume that is size using an non-reserved non-spare disk space in the Disk Group
* ``vxassist make volume size disk1 disk2 ...`` - Same as above but uses space from the specified disks
* ``vxassist make volume size layout=stripe ncols=number`` - Creates a Volume with a single striped Plex that has number of columns using any available disks, minimum of 2 maximum of 8
* ``vxassist make volume size layout=stripe ncols=number disk1 disk2 ...`` - Same as above but only uses storage from the specified disks, try to keep the number of columns the same as the number of disks you specify
* ``vxassist make volume size layout=stripe ncols=number stripeunit=ssize`` - Creates a new Volume with a single striped Plex that uses a stripe size of ssize, ssize should be multiples of 8k
* ``vxassist make volume size layout=stripe ncols=number stripeunit=ssize disk1 disk2 ...`` - Same as above but uses storage from the specified disks
* ``vxassist make volume size layout=raid5`` - Creates a new RAID-5 volume using default settings (3 columns, a stripe size of 16k and a single RAID-5 log Plex)
* ``vxassist make volume size layout=raid5,nlog`` - Same as above but with no log Plex, you can also use the ncol= and stripesize= options to specify the number of columns and/or stripe size to use
* ``vxassist make volume size layout=mirror nmirror=number`` - Creates a new Volume that has number of mirrors
* ``vxassist make volume size layout=mirror,log nmirror=number`` - Same as above but creates a DLR Plex as well
* ``vxassist remove volume volume`` - Deletes volume named *volume* and all of its associated objects


# Plex and Mirror Operations

* ``vxplex att volume plex`` - Attaches plex to volume, the Plexes are synced if necessary
* ``vxplex cp volume new-plex`` - Copies the data from volume to the unassociated Plex new-plex, volume must not be enabled
* ``vxplex det plex`` - Detaches plex from its associated Volume
* ``vxplex dis plex`` - Dissociates plex from its Volume
* ``vxplex mv original-plex new-plex`` - original-plex is an active Plex associated with a Volume and new-plex is a Plex that is not associated with any Volume and is at least the same size as original-plex or larger.  new-plex is associated with the Volume and then original-plex is dissociated.

* ``vxrootmir disk-media-name`` - Creates a mirror of the rootvol volume that can be used as an alternate boot device on disk-media-name, also it seems to create normal UNIX partitions as well

* ``vxmake plex plex sd=sd1,sd2,...`` - Creates a new plex named plex which consistes of subdisks named sd1, sd2 and so on

* ``vxedit -r rm plex`` - Deletes plex and all of its associated Subdisks
* ``vxedit rm plex`` - Deletes plex, it must not be associated and have no associated Subdisks
* ``vxedit rename old-name new-name`` - Renames a Plex, note that its Volume is not renamed

* ``vxbootsetup disk-media-name`` - Use after mirroring a boot disk with vxassist, vxmake, vxplex to make the disk bootable and create standard UNIX VTOC (partitions or slices) and makes the disk bootable

* ``vxassist mirror volume`` - Creates another Plex for volume using space from any available disks
* ``vxassist mirror volume disk1 disk2 ...`` - Same as above but uses space from the specified disk(s)
* ``vxassist remove mirror volume !disk`` - Removes the mirror of volume that resides on !disk

* ``vxmirror -a`` - Mirrors all Volumes in the specified Disk Group until it runs out of space


## Subdisk Operations

* ``vxsd mv old-subdisk new-subdisk1 [ new-subdisk2 ] ...`` - Moves the contents of *old-subdisk* onto one or more new subdisks.  The subdisks must be the same size, *old-subdisk* must be associated with a plex on an active volume, and the new subdisk(s) must not be associated with a plex
* ``vxsd -s size subdisk-to-split subdisk1 subdisk2`` - Splits a *subdisk-to-split* by moving its contents onto two subdisks.  *size* is the amount of data to be put on *subdisk1* and all remaining data is put on *subdisk2*.  If *subdisk-to-split* is associated with a plex the new subdisks are associated with the same plex
* ``vxsd join subdisk1 subdisk2 ... new-subdisk`` - Joins two or more Subdisks (subdisk1 subdisk2 etc) into a single Subdisk (new-subdisk).  The Subdisks being joined must be contiguous on the same disk and associated with the same Plex if they are associated with a Plex
* ``vxsd assoc plex subdisk1 [ subdisk2 ... ]`` - Associates one or more subdisks (*subdisk1* *subdisk2* etc) with a non-RAID5 or non-striped plex (*plex*).
* ``vxsd dis subdisk`` - Dissociates *subdisk* from its plex.  Used when removing a subdisk or dissociating it from its plex so it can be used as part of another plex
* ``vxsd rm subdisk`` - Removes *subdisk* and the space is made available to the disk group it is a part of

* ``vxmake sd sd-name disk01,offset,length`` - Creates a new subdisk named *sd-name* using space from *disk01* that starts at *offset* and is a size of *length* in sectors

* ``vxedit rm subdisk`` - Deletes *subdisk*, it must be dissociated with its plex
* ``vxedit -r rm subdisk`` - Use if the above doesn't work, ensure it is not associated with a plex!
* ``vxedit rename old-name new-name`` - Renames a subdisk from *old-name* to *new-name*, note its disk is not renamed


## Disk Operations

* ``vxdisk list`` - Displays the disk access name and disk media name for all disks in each imported disk group
* ``vxdisk -s list`` - Displays verbose information about each disk on the system that is under VxVM control
* ``vxdisk list disk-name`` - Displays detailed information about *disk-name* which is either a disk nedia name or disk access name

* ``vxdiskconfig`` - Scans for disks that have been added since VxVM has been started.  Note that the OS must see them before this will work.  Generally used when the connection to one or more disks has been interrupted such as a bad SCSI cable or a problem with the SAN, same as ``vxdctl enable``
* ``vxdiskadd device-name`` - Puts *device-name* under VxVM control, you will be prompted for the disk group to add it to and if it should be encapsulated or initialized

* ``vxdisksetup -i device`` - Initializes *device*, puts it under Volume Manager control and into the free disk pool
* ``vxdiskunsetup device`` - Removes a Disk (*device*) from Volume Manager control, it must not be part of a disk group

* ``vxedit rename old-name new-name`` - Renames a disk from *old-name* to *new-name*, note its subdisks are not renamed
* ``vxedit set spare=on disk-media-name`` - Sets *disk-media-name* as a spare
* ``vxedit set spare=off disk-media-name`` - Disables using *disk-media-name* as a spare
* ``vxedit set nohotuse=on disk-media-name`` - Disables *disk-media-name* for hot relocation
* ``vxedit set nohotuse=off disk-media-name`` - Allows *disk-media-name* to be used for hot relocation

* ``vxevac old-media-name new-media-name`` - Evacuates all subdisks that reside on *old-media-name* to *new-media-name*, make sure there is sufficient space on *new-media-name* first!
* ``vxevac disk-media-name`` - Same as above but moves them to any non-reserved disk within the gisk group


## Disk Group Operations

* ``vxdg list`` - Displays a list of all disk groups, the state (enabled or disabled) and disk group ID of each
* ``vxdg list group`` - Displays verbose information about *group*
* ``vxdg free`` - Displays the amount of disk space available on each disk for all disk groups on the system
* ``vxdg nohotuse`` - Displays space on each disk that cannot be used for hot relocation
* ``vxdg destroy`` - Deletes an imported disk group, only one disk must be left and all volumes must be stopped

* ``vxdg adddisk media-name=access-name`` - Adds a disk to a disk group
* ``vxdg spare`` - Displays spare space that can be used for relocating subdisks during recovery
* ``vxdg init name dmname1=daname1 dmname2=daname2 ...`` - Creates the new gisk group name, an example of dmname1=daname1 is newdg01=c1t0d0

* ``vxdg deport group`` - Deports *group*, it is NOT automatically imported when the system is rebooted
* ``vxdg deport -h host group`` - Deports *group* on *host* and it is automatically imported by host next time host reboots
* ``vxdg -n new-name deport old-name`` - Exports the disk group *old-name* and renames it to *new-name*

* ``vxdg import disk-group`` - Imports *disk-group*
* ``vxdg -n new-name import old-name`` - Imports the Disk Group *old-name* and renames it to *new-name*
* ``vxdg -t -n new-name import old-name`` - Same as above but the *new-name* is only temporary, usually used when importing a second rootdg for recovery


## Licensing

* ``hostid`` - Displays the Host ID of the system.  Veritas licenses are based off a system's Host ID and system type (see below)
* ``uname -i`` - The system type (Ultra 2, Ultra 60, etc), also required by Veritas to generate a license key

* ``vxlicense -u`` - Displays the system's Host ID in hex
* ``vxlicense -p`` - Displays currently licensed products
* ``/usr/lib/vxfs/bin/vxliccheck -pv`` - Verifies the licenses for installed products are valid
* ``vxlicense -c`` - Prompts for a license key.  Enter it to enable VxVM or any additional features
* ``vxdctl license`` - Displays currently installed licenses based on known licensing information
* ``vxdctl license init`` - Forces VxVM to scan for new licenses, **WILL EXPIRE OLD EXPIRED LICENSES**


## Managing VxVM Tasks

``vxtask`` shows the progress of tasks (subdisk moves, RAID rebuilds, volume grows, etc).

* ``vxtask list`` - Displays tasks currently in progress and the % completed
* ``vxtask abort task-id`` - Aborts *task-id*
* ``vxtask pause task-id`` - Pauses *task-id*
* ``vxtask resume task-id`` - Resumes *task-id* that was paused with ``vxtask pause``
* ``vxtask monitor task-id`` - Displays ``vmstat 1`` type output for *task-id*, displays the task as EXITED when it completes


## Replacing a Failed Disk

Use this method when the OS can still see the disk (bad blocks, etc):

1. Use ``vxdiskadm`` and choose option 4, Remove a Disk for Replacement
2. Physically remove the failed disk, reattach and power on the new disk.  Make sure it has the same SCSI ID as the original disk
3. Use the ``format`` command to verify the system can see the disk and write a label to it
4. Execute the ``vxdctl enable`` or ``vxdiskconf`` command to have VxVM scan for new disks
5. Run ``vxdiskadm`` and choose option 5, Replace a Failed or Removed Disk
6. Go though the steps.  If it asks you to encapuslate or initialize the disk initialize it

Use this method when the OS can no longer see the failed disk:
  
1. Physically remove the failed disk, reattach and power on the new disk.  Ensure it has the same SCSI ID as the old one
2. Use ``format`` to write a label to the new disk.  If format doesn't see the disk go to the next step
3. Run ``drvconfig`` and ``disks`` on Solaris 2.6 or ``devfsadm`` on Solaris 7 and later and run format again
4. Run ``vxdiskadm`` and use option 1, Add or initialize one or more disks
5. Follow the menus and add the disk and exit ``vxdiskadm``.  If it asks, tell it to initialize the new disk
6. Use ``vxassist mirror volume`` to re-mirror the volumes one at at time
7. Use ``vxbootsetup`` on the new disk to make it bootable


## Growing and Shrinking Volumes

Sizes can be specified with the b, k, m and g modifiers which are blocks, kilobytes, megabytes and gigabytes respectively.  For example, 3g is 3 gigabytes and 700m is 700 megabytes.  No letter is specified the size is assumed to be 512 byte blocks/sectors

* You can use the ``vxassist maxgrow volume`` command to see how much you can grow *volume* by
* Using ``vxresize`` keeps data accessible at all times and automatically extends the file system
* UFS file systems cannot be shrunk
* ``vxassist`` kinda sucks because it doesn't resize the file system, you must do it manually

* ``vxassist maxgrow volume`` - Displays the size in megs/gigs and sectors that *volume* can be grown by given the volume's layout
* ``vxdg free`` - Displays the amount of free space on each disk in sectors and displays the offset where free space starts on each disk

* ``vxresize volume size`` - Grows *volume* to size
* ``vxresize volume size disk1 disk2 ...`` - Same as above but specifies the disk(s) used to grow the volume.  Note that at least one disk must be specified for each plex (eg: 3 plexs = 3 disks)
* ``vxresize volume +size`` - Grows volume by size, resizes the file system automatically
* ``vxresize volume -size`` - Shrinks volume by size, resizes the file system automatically


## Building Volumes Using vxmake

Follow the steps below to manually create a volume using ``vxmake``.  You might want to use ``vxassist`` instead, it's much easier.
  
1. ``vxdg free`` # determine the offsets
2. ``vxdg init customdg disk01=c0t2d0s2 disk02=c0t3d0s2`` # creates the disk group named *customdg* and adds the disks
3. ``vxmake sd subdisk01 disk01,0,10000`` # creates the first subdisk
4. ``vxmake sd subdisk02 disk02,0,10000`` # creates the second subdisk
5. ``vxmake plex plex01 sd=subdisk01`` # creates the first plex
6. ``vxmake plex plex02 sd=subdisk02`` # creates the second plex
7. ``vxmake -U fsgen vol data plex=plex01,plex02`` # creates the volume
8. ``vxvol start data`` # starts the volume
9. ``newfs device`` # create a file system
10. ``mount device mount-point`` # mount the volume
11. Update ``/etc/vfstab``


## Misc Sweet Commands

* ``/etc/vx/diag.d/vxdmpinq raw-device`` - Displays the vendor, product ID, serial number and revision of the disk
* ``vxstat -s -ff plex`` - Displays I/O errors on Subdisks belonging to plex