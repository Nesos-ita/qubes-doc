---
layout: doc
title: Copying Files between qubes
permalink: /doc/copying-files/
redirect_from:
- /en/doc/copying-files/
- /doc/CopyingFiles/
- /wiki/CopyingFiles/
---

Copying files and folders between qubes
=============================

Qubes also supports secure copying of files and folders between qubes.
These instructions refer to file(s) but equally apply to copying folders.

In order to copy file(s) from qube A to qube B, follow these steps:

GUI
---

1.  Open file manager in the source qube (qube A), choose file(s) that you wish to copy, and right click on the selection, and choose `Copy to another AppVM`

1.  A dialog box will appear asking for the name of the destination qube (qube B). 

1.  A confirmation dialog box will appear(this will be displayed by Dom0, so none of the qubes can fake your consent). After you click ok, qube B will be started if it is not already running, the file copy operation will start, and the files will be copied into the following folder in qube B:

-   `/home/user/QubesIncoming/<source>`

1.  You can now move them whenever you like in the qube B filesystem using the file manager there.

CLI
---

[qvm-copy-to-vm](/doc/vm-tools/qvm-copy-to-vm/)

On inter-qube file copy security
----------------------------------

The scheme is *secure* because it doesn't allow other qubes to steal the files that are being copying, and also doesn't allow the source qube to overwrite arbitrary file on the destination qube. Also, Qubes file copy scheme doesn't use any sort of virtual block devices for file copy -- instead we use Xen shared memory, which eliminates lots of processing of untrusted data. For example, the receiving qube is *not* forced to parse untrusted partitions or file systems. In this respect our file copy mechanism provides even more security than file copy between two physically separated (air-gapped) machines!

However, one should keep in mind that performing a data transfer from *less trusted* to *more trusted* qubes can always be potentially insecure, because the data that we insert might potentially try to exploit some hypothetical bug in the destination qube (e.g. a seemingly innocent JPEG that we copy from an untrusted qube might contain a specially crafted exploit for a bug in JPEG parsing application in the destination qube). This is a general problem and applies to any data transfer between *less trusted to more trusted* qubes. It even applies to the scenario of copying files between air-gapped machines. So, you should always copy data only from *more trusted* to *less trusted* qubes.

See also [this article](http://theinvisiblethings.blogspot.com/2011/03/partitioning-my-digital-life-into.html) for more information on this topic, and some ideas of how we might solve this problem in some future version of Qubes.

You may also want to read how to [revoke "Yes to All" authorization](/doc/Qrexec/#revoking-yes-to-all-authorization)
