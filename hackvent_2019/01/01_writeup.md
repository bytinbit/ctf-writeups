# HV19.01 censored

**Task:** I got this little image, but it looks like the best part got censored on the way. Even the tiny preview icon looks clearer than this! Maybe they missed something that would let you restore the original content?

**Flag:** `HV19{just-4-PREview!}`

## Research

Images have been successfully deblurred before, so I first searched for some techniques to achieve this. This was the wrong direction though. 

**Solution Version 1: binwalk**

`binwalk` shows several images in one file:

``` bash
$ binwalk 01.jpg                        
DECIMAL       HEXADECIMAL     DESCRIPTION

0             0x0             JPEG image data, EXIF standard
12            0xC             TIFF image data, little-endian 
							  offset of first image directory: 8
332           0x14C           JPEG image data, JFIF standard 1.01

```

The command `binwalk -eM 01.jpg` doesn't extract the files as expected. 
However, tweaking the command does the trick: ` $ binwalk --dd=".*" 01.jpg`

It extracts three files, one of which is a tiny preview file, scanning the QR-Code reveals the flag.

**Solution Version 2: exiftool**

`exiftool -b -ThumbnailImage 01.jpg > my_thumbnail.jpg` achieves the same.



