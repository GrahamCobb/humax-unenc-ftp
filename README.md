# humax-unenc-ftp

Remove ENC flag from Humax recordings remotely, using FTP access.

This script uses FTP to look through all the Humax recordings and clear the ENC flag.

Note, there is a well known program called `Foxy` that clears the ENC flag on local
files.  This script is not related to that program but does the same job for recordings
on the Humax itself.

This is useful on the HDR-2000T, which sets the ENC flag on all HD recordings and then refuses
to serve them over DLNA or copy them to USB disks.

## Detailed operation

The script looks through all the files below a certain point in the FTP tree, looking for
`.hmt` files.  Each time it finds one, it reads the file to check if it is marked as ENC.

If so, it backs up the `.hmt` file (to `.hmt.bck`) and rewrites the .hmt flag with the ENC
flag clear.  Then it continues searching for `.hmt` files.

## Effect on encryption

This has no effect on encryption: the files on disk (and available via FTP) are still encrypted.
This only affects the Humax `ENC` flag associated with the programme.

Clearing the `ENC` flag may mean the Humax will be willing to decrypt files in more cases.

## Effect on Media listing

As soon as the file has been rewritten, the Humax Media display shows the programmes without
the ENC flag.

## Effect on DLNA server

???

## Effect on copying programmes to USB disks

???

## Requirements

1. Humax PVR must have a network connection, with a known IP address.

2. Humax must be on (not just in standby) when the script runs.
You may want to run the script frequently so it can catch the Humax
when it is on.

3. FTP must be enabled on the Humax.

## Notices
Copyright (c) 2014 Graham R. Cobb.
This software is distributed under the GPL (see the copyright notices and the LICENSE file).

`humax-unenc-ftp` is free software; you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation; either version 2 of the License, or
(at your option) any later version.

`backup-humax` is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

**IMPORTANT NOTES:** Humax is a trademark of Humax Co. Ltd.
This page and software is not created, endorsed, reviewed, approved or in any other
way associated with Humax Co. Ltd.

