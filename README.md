bluebox
=======

A backup experience for supported operating systems.

Inspired by Apple OS X backup technology.

My intention is to create a simple way to backup files continiously, providing the user with the ability to "travel through time", viewing and restoring individual files, or full operating sytem snapshots.

The systems I would like to implement this for would be (Ubuntu) GNU/ Linux, and maybe FreeBSD. If I can get it to do the things I want to change about Apple's solution, it would be awesome to build an OS X compatible client.

I would like to initially implement a client for a locally attached USB drive, and soon after for a network location.

Network locations could be something as simple as a USB drive plugged into a home access point, connected by AFP, NFS, SMB, FTP, etc.

Something I have struggled a bit with in the past using Apple's solution is creating an initial backup to a local drive, then moving that drive to a network share point on another piece of Apple hardware (shared drive on another computer, shared drive on an access point, etc) for incremental updates.

Additionally, wouldn't it be cool to be able to restore your operating system configuration and files you care about, but without wasting the netwotk bandwidth and hosted space of a full disk backup? I think so. Because of this, I'd like to implement a version of the software that can help you replicate your installation - but I'd start by only supporting "vanilla" installs.

By supporting only vanilla installs, I mean creating a "profile" for a backup of a freshly installed system, based on the choices made by the user while installing. In the beginning I will only support the preferences I have, but I am open minded in how to efficiently represent profiles and restore data for things like different languages.

The main thing I am concerned with is "vanilla" package config.

If bluebox-lite is configured for a system, packages and system preferences will be saved as the user modifies stuff, and user data will be uploaded when it is original content.
It will be tricky to keep maintained.

Anyway. I want this to be my perfect backup solution. If I googled around long enough, I could probably find other solutions, but this is something I want to code from scratch more or less, for my own edification and gratification.

So, at this point we have:
bluebox localdrive
bluebox networkdrive
bluebox network-lite

Additionally, I would like to create a USB, CD, and PXEboot-able distribution for creating backups fast (no other OS overhead), and restoring file and OS states/ snapshots.
Let's call this:
bluebox vortextravel
(referencing the "time vortex")

The interface should be a fullscreen version of the desktop GUI application, with an option for terminal based (for non-GUI operating system installations)

In addition to all the crazy stuff above, I'd like to also implement a server.
The server would be a piece of software (deployable as a lightweight bootable server similar to FreeNas) that has local access to the backup drive, and has files "deposited" or "withdrawn" by client software.

This would allow you to deploy a home backup solution that puts less stress on your local machines - something that the Apple competition allows with a specialized device.

Additionally, it would allow a hosting provider to deploy a private (or public - oh my!) instance of bluebox, such that a user could remotely access all files.

The idea for a full OS backup is to create something that has a high initial cost, low intermediate maintenance compute load cost, and a low reconstruction and redeployment cost - who wants to wait hours to restore a backup?

A long time ago, I learned about block-copying drives for backup and restore, and it was pretty mind opening.

I'd like to traverse all (pernamently) mounted partitions, go down into all directories, and scan and compress files.

Over time, the scanning process will allow for indexing, and will integrate with the OS more over time to be used as an indexing and appliation launcher system.

Files with inapprpriate entropy will not be compressed, because the time tradeoff would be ineffective.

A disk image is created for each appropriately large directory (small directories might waste overhead), and over time the disk images are re-optomized and balanced for quick restore.
The disk images are arranged heirarchially for upload, but as a single flat image for quick restore of specific snapshotted moments, but also addresses are indexed for files and file versions, such that restore of an individual file is also efficient.
