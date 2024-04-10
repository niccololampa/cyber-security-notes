## Tunn3l V1s10n Solution

This is the solution for [picoCTF's Tunn3l V1s10n challenge](https://play.picoctf.org/practice/challenge/112?page=2) forensics problem.

![Screenshot 2024-04-10 at 12 33 38 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/81978160-ec1d-4775-889e-06316c2dc441)

This problem was taken from the picoCTF 2021 and the solution will be discussed below. So proceed with caution.

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
open tunn3l_v1s10n
```

![Screenshot 2024-04-10 at 12 58 34 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/9c886ff8-24cd-45ab-b433-e8df3e791f63)

No image comes out. But there is an error stating that **BMPImage has unsupported header size**

Maybe it is a wrong file format so let's try coverting it to a different format. 

```bash
convert -quality 100 tunn3l_v1s10n copy.bmp
convert -quality 100 tunn3l_v1s10n copy.jpg
convert -quality 100 tunn3l_v1s10n copy.png
```

![Screenshot 2024-04-10 at 1 17 56 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/ee1e68ce-9b7a-4caf-b90e-e4e9a2fba791)

Although the error has disappeared and  now able to view the image there is still no flag.

Looking at the hint **Weird that it won't display right...**, we see that the image has something weird in it. So let's experiment using Gimp (photo editor). Experimented with color balance etc (because of the weird colors) but still nothing. 



Looking at the information we have derived we know that image dimension (wxh) is 1134x306= 337,306 however the total size of file 2,893,454 which is much larger. 

Probably we can modify with the image dimension as  there might be some image data not being shown.

Let's open the file

```bash
hexeditor tunn3lv1s10n
```

Now we refer to [BMP documentation](https://en.wikipedia.org/wiki/BMP_file_format) to figure out how to modify the dimension of image. 

From the documentation we found in `BITMAPINFOHEADER` that image width and height can be edited at hex offset 12 and 16. 


![Screenshot 2024-04-10 at 10 30 00 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/e78ca2b0-dfd4-4e7e-90fc-137c2326750f)

So going to our hexeditor we find the hex values of width at offset 12 to be `6E 04` (integer value 1134) and height at offset 16 to be `32 01` (integer value 306). 
![Screenshot 2024-04-10 at 10 37 18 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/bd6de01e-cea5-4745-b55d-be11107820f2)

Let's try modifying height to be the same as width `6E 04` and saving the file as `tunn3l_v1s10n_modified`

![Screenshot 2024-04-10 at 10 39 53 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/1939c1f1-68a1-4d1b-ae4a-928ad94b2971)


Now let's convert the `tunn3l_v1s10n_modified`

```bash
convert -quality 100 tunn3l_v1s10n_modified copy_modfied.bmp
```
However this results in an error. It seems we exceeded the image data with the height hex value. 
![Screenshot 2024-04-10 at 11 32 31 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/2628c6ce-876f-4319-85c6-3c6c2458e3f9)

Now let's just try incrementing change height to `32 02` then rerun the commands above. 

```bash
convert -quality 100 tunn3l_v1s10n_modified copy_modfied.bmp
```

Certainly showed a little bit more of the image. 
![Screenshot 2024-04-10 at 11 35 42 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/e95507f5-659f-4a47-ae33-b809a72b9fb7)

Now let's try `32 03` for the height. 

![Screenshot 2024-04-09 at 1 23 46 AM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/6b30de11-6628-4296-961a-23ce1c7f8709)

Now the image has shown the portion which has the flag. 


**Flag: picoCTF{qu1t3_a_v13w_2020}**

Until next time. Keep learning.

Stay stoked and code. :)


I hope you can voluntarily [Buy Me A Coffee](https://www.buymeacoffee.com/thedatalife) if you found this article useful and give additional support for me to continue sharing more content for the community. :)

**Thank you very much. :)**

