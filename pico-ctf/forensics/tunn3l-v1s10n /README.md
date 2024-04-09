## Tunn3l V1s10n Solution

This is the solution for [picoCTF's Tunn3l V1s10n challenge](https://play.picoctf.org/practice/challenge/112?page=2) forensics problem.

![Screenshot 2024-04-10 at 12 33 38 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/81978160-ec1d-4775-889e-06316c2dc441)

Here we are able to download a file called `tunn3l_v1s10n`.

Let's investigate the file first by running 

```bash
file tunn3l_v1s10n
stat tunn3l_v1s10n
```

![Screenshot 2024-04-10 at 12 46 41 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/6eccbdfc-42d5-47d6-8e58-b3caa0176ea5)

Nothing specifically interesting yet. 

So next we look at the metadata by running 

```bash
exiftool -listf tunn3l_v1s10n
```

```bash
ExifTool Version Number         : 12.67
File Name                       : tunn3l_v1s10n
Directory                       : .
File Size                       : 2.9 MB
File Modification Date/Time     : 2024:04:07 04:30:51-07:00
File Access Date/Time           : 2024:04:07 04:32:34-07:00
File Inode Change Date/Time     : 2024:04:07 04:32:34-07:00
File Permissions                : -rw-r--r--
File Type                       : BMP
File Type Extension             : bmp
MIME Type                       : image/bmp
BMP Version                     : Unknown (53434)
Image Width                     : 1134
Image Height                    : 306
Planes                          : 1
Bit Depth                       : 24
Compression                     : None
Image Length                    : 2893400
Pixels Per Meter X              : 5669
Pixels Per Meter Y              : 5669
Num Colors                      : Use BitDepth
Num Important Colors            : All
Red Mask                        : 0x27171a23
Green Mask                      : 0x20291b1e
Blue Mask                       : 0x1e212a1d
Alpha Mask                      : 0x311a1d26
Color Space                     : Unknown (,5%()
Rendering Intent                : Unknown (826103054)
Image Size                      : 1134x306
Megapixels                      : 0.347
```

Here we discover that it is a bmp image. Let's try opening it.

```bash
open tunn3lv1s1on
```

![Screenshot 2024-04-10 at 12 58 34 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/9c886ff8-24cd-45ab-b433-e8df3e791f63)

No image comes out. But there is an error stating that **BMPImage has unsupported header size**

Maybe it is a wrong file format so let's try coverting it to a different format. 

```bash
convert -quality 100 tunn3lv1s1on copy.bmp
convert -quality 100 tunn3lv1s1on copy.jpg
convert -quality 100 tunn3lv1s1on copy.png
```

![Screenshot 2024-04-10 at 1 17 56 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/ee1e68ce-9b7a-4caf-b90e-e4e9a2fba791)

Although the error has disappeared and  now able to view the image there is still no flag.

Looking at the hint **Weird that it won't display right...**, we see that the image has something weird in it. So let's experiment using Gimp (photo editor). Experimented with color balance etc (because of the weird colors) but still nothing. 

![Screenshot 2024-04-09 at 1 23 46 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/6b30de11-6628-4296-961a-23ce1c7f8709)





**Flag: picoCTF{qu1t3_a_v13w_2020}**


