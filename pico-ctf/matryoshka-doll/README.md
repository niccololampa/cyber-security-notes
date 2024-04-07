
## Matryoshka Doll Solution

This is the solution for [picoCTF's Matryoshka doll challenge](https://play.picoctf.org/practice/challenge/129?page=2) forensics problem.

![Screenshot 2024-04-07 at 6 27 37 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/de252511-e9b5-49a7-8728-060324dc8426)

This problem was taken from the picoCTF 2021 and the solution will be discussed below. So proceed with caution.

First let's dowload the image file which is `dolls.jpg`. Shown below:

```bash
open dolls.jpg
```

![Screenshot 2024-04-07 at 6 03 05 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/93254b21-97fd-4134-b484-99e8e62d9a6f)

Let's investigate the file by reading its metadata. We will use a tool called `exiftool`

```bash
exiftool -listf dolls.jpg
```

```bash
Supported file extensions:
  360 3FR 3G2 3GP 3GP2 3GPP 7Z A AA AAE AAX ACFM ACR AFM AI AIF AIFC AIFF AIT
  AMFM APE APNG ARQ ARW ASF AVI AVIF AZW AZW3 BMP BPG BTF CHM CIFF COS CR2 CR3
  CRM CRW CS1 CSV CUR CZI DC3 DCM DCP DCR DFONT DIB DIC DICM DIVX DJV DJVU DLL
  DNG DOC DOCM DOCX DOT DOTM DOTX DPX DR4 DS2 DSS DV DVB DVR-MS DYLIB EIP EPS
  EPS2 EPS3 EPSF EPUB ERF EXE EXIF EXR EXV F4A F4B F4P F4V FFF FIT FITS FLA
  FLAC FLIF FLIR FLV FPF FPX GIF GLV GPR GZ GZIP HDP HDR HEIC HEIF HIF HTM HTML
  ICAL ICC ICM ICO ICS IDML IIQ IND INDD INDT INSP INSV INX ISO ITC J2C J2K JNG
  JP2 JPC JPE JPEG JPF JPG JPM JPS JPX JSON JXL JXR K25 KDC KEY KTH LA LFP LFR
  LIF LNK LRV M2T M2TS M2V M4A M4B M4P M4V MACOS MAX MEF MIE MIF MIFF MKA MKS
  MKV MNG MOBI MODD MOI MOS MOV MP3 MP4 MPC MPEG MPG MPO MQV MRC MRW MTS MXF
  NEF NEWER NKSC NMBTEMPLATE NRW NUMBERS O ODB ODC ODF ODG ODI ODP ODS ODT OFR
  OGG OGV ONP OPUS ORF ORI OTF PAC PAGES PBM PCD PCT PCX PDB PDF PEF PFA PFB
  PFM PGF PGM PICT PLIST PMP PNG POT POTM POTX PPAM PPAX PPM PPS PPSM PPSX PPT
  PPTM PPTX PRC PS PS2 PS3 PSB PSD PSDT PSP PSPFRAME PSPIMAGE PSPSHAPE PSPTUBE
  QIF QT QTI QTIF R3D RA RAF RAM RAR RAW RIF RIFF RM RMVB RPM RSRC RTF RV RW2
  RWL RWZ SEQ SKETCH SO SR2 SRF SRW SVG SWF THM THMX TIF TIFF TORRENT TS TTC
  TTF TUB TXT VCARD VCF VNT VOB VRD VSD WAV WDP WEBM WEBP WMA WMV WOFF WOFF2
  WPG WTV WV X3F XCF XHTML XLA XLAM XLS XLSB XLSM XLSX XLT XLTM XLTX XMP ZIP
ExifTool Version Number         : 12.67
File Name                       : dolls.jpg
Directory                       : .
File Size                       : 652 kB
File Modification Date/Time     : 2024:04:06 10:35:09-07:00
File Access Date/Time           : 2024:04:07 03:02:45-07:00
File Inode Change Date/Time     : 2024:04:07 02:53:19-07:00
File Permissions                : -rw-r--r--
File Type                       : PNG
File Type Extension             : png
MIME Type                       : image/png
Image Width                     : 594
Image Height                    : 1104
Bit Depth                       : 8
Color Type                      : RGB with Alpha
Compression                     : Deflate/Inflate
Filter                          : Adaptive
Interlace                       : Noninterlaced
Profile Name                    : ICC Profile
Profile CMM Type                : Apple Computer Inc.
Profile Version                 : 2.1.0
Profile Class                   : Display Device Profile
Color Space Data                : RGB
Profile Connection Space        : XYZ
Profile Date Time               : 2020:06:09 12:08:45
Profile File Signature          : acsp
Primary Platform                : Apple Computer Inc.
CMM Flags                       : Not Embedded, Independent
Device Manufacturer             : Apple Computer Inc.
Device Model                    : 
Device Attributes               : Reflective, Glossy, Positive, Color
Rendering Intent                : Perceptual
Connection Space Illuminant     : 0.9642 1 0.82491
Profile Creator                 : Apple Computer Inc.
Profile ID                      : 0
Profile Description             : Display
Profile Description ML (hr-HR)  : LCD u boji
Profile Description ML (ko-KR)  : 컬러 LCD
Profile Description ML (nb-NO)  : Farge-LCD
Profile Description ML (hu-HU)  : Színes LCD
Profile Description ML (cs-CZ)  : Barevný LCD
Profile Description ML (da-DK)  : LCD-farveskærm
Profile Description ML (nl-NL)  : Kleuren-LCD
Profile Description ML (fi-FI)  : Väri-LCD
Profile Description ML (it-IT)  : LCD colori
Profile Description ML (es-ES)  : LCD color
Profile Description ML (ro-RO)  : LCD color
Profile Description ML (fr-CA)  : ACL couleur
Profile Description ML (uk-UA)  : Кольоровий LCD
Profile Description ML (he-IL)  : LCD צבעוני
Profile Description ML (zh-TW)  : 彩色LCD
Profile Description ML (vi-VN)  : LCD Màu
Profile Description ML (sk-SK)  : Farebný LCD
Profile Description ML (zh-CN)  : 彩色LCD
Profile Description ML (ru-RU)  : Цветной ЖК-дисплей
Profile Description ML (en-GB)  : Colour LCD
Profile Description ML (fr-FR)  : LCD couleur
Profile Description ML (hi-IN)  : रगन LCD
Profile Description ML (th-TH)  : LCD ส
Profile Description ML (ca-ES)  : LCD en color
Profile Description ML (en-AU)  : Colour LCD
Profile Description ML (es-XL)  : LCD color
Profile Description ML (de-DE)  : Farb-LCD
Profile Description ML          : Color LCD
Profile Description ML (pt-BR)  : LCD Colorido
Profile Description ML (pl-PL)  : Kolor LCD
Profile Description ML (el-GR)  : Έγχρωμη οθόνη LCD
Profile Description ML (sv-SE)  : Färg-LCD
Profile Description ML (tr-TR)  : Renkli LCD
Profile Description ML (pt-PT)  : LCD a Cores
Profile Description ML (ja-JP)  : カラーLCD
Profile Copyright               : Copyright Apple Inc., 2020
Media White Point               : 0.94955 1 1.08902
Red Matrix Column               : 0.51099 0.23955 -0.00104
Green Matrix Column             : 0.29517 0.69981 0.04224
Blue Matrix Column              : 0.15805 0.06064 0.78369
Red Tone Reproduction Curve     : (Binary data 2060 bytes, use -b option to extract)
Video Card Gamma                : (Binary data 48 bytes, use -b option to extract)
Native Display Info             : (Binary data 62 bytes, use -b option to extract)
Chromatic Adaptation            : 1.04861 0.02332 -0.05034 0.03018 0.99002 -0.01714 -0.00922 0.01503 0.75172
Make And Model                  : (Binary data 40 bytes, use -b option to extract)
Blue Tone Reproduction Curve    : (Binary data 2060 bytes, use -b option to extract)
Green Tone Reproduction Curve   : (Binary data 2060 bytes, use -b option to extract)
Exif Byte Order                 : Big-endian (Motorola, MM)
X Resolution                    : 144
Y Resolution                    : 144
Resolution Unit                 : inches
User Comment                    : Screenshot
Exif Image Width                : 594
Exif Image Height               : 1104
Pixels Per Unit X               : 5669
Pixels Per Unit Y               : 5669
Pixel Units                     : meters
XMP Toolkit                     : XMP Core 5.4.0
Apple Data Offsets              : (Binary data 28 bytes, use -b option to extract)
Warning                         : [minor] Trailer data after PNG IEND chunk
Image Size                      : 594x1104
Megapixels                      : 0.656
```

Nothing seems particularly interesting. 

Based on the hint we know that there is a file that is hiding within the image file. 

This can be done through what is called as **steganography** 

Basically you embed a zip file in another file. 

So to retrive that file we do the following. 

Rename the file into a zip file and uncompress it. 

```bash
mv dolls.jpg dolls.zip
unzip dolls.zip
```

We are successful in uncompressing the file that is embeded within `dolls.jpg` called `base_images/2_c.jpg`.

![Screenshot 2024-04-07 at 6 07 52 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/cd081369-ffaa-4d20-911e-a18dcd873fae)

Also opening the image we find out that it as a smaller doll image compared to the original. Just like how it is when you open a Russian Matryoshka doll.

![Screenshot 2024-04-07 at 6 11 45 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/c4d208a5-e40b-4b1c-9525-aa91132acebd)


Since we haven't found the flag, let us repeat the process of renaming and uncompressing

```bash
mv 2_c.jpg 2_c.zip
unzip 2_c.zip
```
![Screenshot 2024-04-07 at 6 15 43 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/fcdf3c38-a00e-4ad9-ac39-dc31d25926e9)

Another file called as `base_images/3_c.jpg` and a smaller doll compared to before.

![Screenshot 2024-04-07 at 6 21 06 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/ee951d9f-766a-476b-972b-7f8278de4eed)


```bash
mv 3_c.jpg 3_c.zip
unzip 3_c.zip
```

![Screenshot 2024-04-07 at 6 20 02 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/2731bf82-dfe5-44f9-95d8-4da519b87e4a)

We are pretty deep and a new file has emerged `base_images/4_c.jpg` and yet another smaller doll. 

![Screenshot 2024-04-07 at 6 17 28 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/a77bde78-0a60-450b-a158-ece2b801117e)

Let's repeat and I hope this is the last one.

```bash
mv 4_c.jpg 4_c.zip
unzip 4_c.zip
```

Finally a `flag.txt` has been uncompressed. 


![Screenshot 2024-04-07 at 6 23 51 PM](https://github.com/niccololampa/cyber-security-notes/assets/37615906/ea55a0df-ef90-4ba9-a58c-326502a90964)

**Answer :picoCTF{bf6acf878dcbd752f4721e41b1b1b66b}**

Until next time. Keep learning.

Stay stoked and code. :)


I hope you can voluntarily [Buy Me A Coffee](https://www.buymeacoffee.com/thedatalife) if you found this article useful and give additional support for me to continue sharing more content for the community. :)

**Thank you very much. :)**
