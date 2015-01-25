# humax-unenc-ftp

Remove ENC flag from Humax recordings remotely, using FTP access.

There are two tools: `humax-unenc-ftp` and `hmt2k`.

# humax-unenc-ftp

This script uses FTP to look through all the Humax recordings and clear the ENC
flag.

Note, there is a well known program called `Foxy` that clears the ENC flag on
local files.  This script is not related to that program but does the same job
for recordings on the Humax itself.

This is useful on the HDR-2000T, which sets the ENC flag on all HD recordings
and then refuses to serve them over DLNA or copy them to USB disks.

## Usage
```
    humax-unenc-ftp [options] <address> [<directory> ...]

    humax-unenc-ftp -?|--help|--man|--version

     Commands:

      -?, --help                    brief help message
      --man                         full documentation
      --version                     script version

     Options:

      -r, --rename          Rename files after removing flag
      -p, --password=<pass> Specify FTP password (default 0000)
      -u, --user=<user>     Specify FTP username (default humaxftp)
      -v, --verbose[=N]     Increment or set verbosity level (0 - quiet, 1 - info, 2 - progress, 3 - debug, 4 - debug FTP)
```

The default directory is currently "My Video/Test", although it will change in
a future release.

## Detailed operation

The script looks through all the files below a certain point in the FTP tree,
looking for `.hmt` files.  Each time it finds one, it reads the file to check
if it is marked as ENC.

If so, it backs up the `.hmt` file (to `.hmt.bck`) and rewrites the `.hmt` flag
with the ENC flag clear.

After updating the flag, the script optionally renames all the files associated
with the recording (to add a hyphen at the end) to trigger a DLNA media server
update.  This feature is considered experimental as it can cause some problems
if the files are in use (recording, playback, or suspended playback).

## Effect on encryption

This has no effect on encryption: the files on disk (and available via FTP) are
still encrypted. This only affects the Humax `ENC` flag associated with the
programme.

Clearing the `ENC` flag may mean the Humax will be willing to decrypt files in
more cases.

## Effect on Media listing

As soon as the file has been rewritten, the Humax Media display shows the
programmes without the ENC flag.

## Effect on DLNA media server

The DLNA media server does not serve recordings marked as ENC.  Even if the flag
is later cleared, the recording is not served.

However, if the recording file (and associated files) names are changed in some
way, the media server will eventually rebuild its index and serve the recording
(which can be played on a DLNA media player). It is not clear exactly what
triggers the rebuild but it seems to happen overnight (although it may also
require going into and out of standby).  More testing on this would be welcomed.

The optional file renaming feature is designed to trigger this update. This
does not change the name in the Media listing but can cause some small problems.

## Effect on copying programmes to USB disks

??? Please let me know if you have tried this.

## Requirements

1. Humax PVR must have a network connection, with a known IP address.

2. Humax must be on (not just in standby) when the script runs.
You may want to run the script frequently so it can catch the Humax
when it is on.

3. FTP must be enabled on the Humax.

# hmt2k

Decode content of a `.hmt` file.

This is an open source re-implementation of Raydon's `hmt` program, to
display the content of a .hmt file locally or on the Humax. Although
created for HDR-2000T users, I believe it should also work with other
models.

## Usage
```
    hmt2k <file> | <url> ...

```

Note: the .hmt file can be specified using either a local filespec or a URL
(for example `ftp://humaxftp:0000@humax/My Video/file.hmt`).

## HMT data

The original research for the meanings of the HMT data was done by raydon.
Many thanks to raydon for publishing that research.

I have made some small changes to raydon's analysis to make it work better
on the .hmt files I encounter.  I do not know if these are mistakes by me,
by raydon, different HMT layout versions or just the results of different
data.  Feedback on errors in the HMT format or suggestions for the meanings
of currently not understood fields are welcome.

I would particularly like suggestions on why some strings (for example
`Schedule information`) in some files are prefixed with the characters
`i7` but in other files they are not.

# Notices
Copyright (c) 2014, 2015 Graham R. Cobb.
This software is distributed under the GPL (see the copyright notices and the
LICENSE file).

`humax-unenc-ftp` and `hmt2k` are free software; you can redistribute them
and/or modify them under the terms of the GNU General Public License as
published by the Free Software Foundation; either version 2 of the License,
or (at your option) any later version.

`humax-unenc-ftp` and `hmt2k` are distributed in the hope that they will be
useful, but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

**IMPORTANT NOTES:** Humax is a trademark of Humax Co. Ltd.
This page and software is not created, endorsed, reviewed, approved or in any
other way associated with Humax Co. Ltd.

