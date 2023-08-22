# Steganography

## The basic

Finding the hidden flag in a file.

### Useful command (Where {} = filename)

```
binwalk {}
binwalk -e {}
strings {} / cat {}
xxd {}
unzip {}
gunzip {}
tar -xvf {}
```

## Image

1. [StegSolve](https://github.com/zardus/ctf-tools/blob/master/stegsolve/install)

```
java -jar stegsolve.jar
```

1. Steghide (JPEG, BMP, WAV and AU)

```
steghide extract -sf [FILE.JPG]
```

1. [Stegcracker](https://github.com/Paradoxis/StegCracker)

```
stegcracker [FILE.JPG] [DICTIONARY.TXT]
```

1. [Steg\_brute](https://github.com/Diefunction/stegbrute)

```
python steg_brute.py -b -d [ROCKYOU.TXT] -f [FILE].JPG
```

1. [zsteg (PNG)](https://github.com/zed-0xff/zsteg)

```
zsteg -a [FILE].png
```

1. [zbarimg (QR Code)](http://manpages.ubuntu.com/manpages/bionic/man1/zbarimg.1.html)

```
zbarimg [FILE].png/jpg
```

1. [Stegseek](https://github.com/RickdeJager/stegseek)

```
stegseek -sf [FILE]
```

## Compressed Files

1. [zcat](https://linux.die.net/man/1/zcat)

```
zcat [FILE.tar.gz] > FILE.txt
```

1. unzip

```
unzip [FILE.ZIP]
```

1. 7zip

```
7z e [FILE.ZIP]
```

1. [fcrackzip](https://www.cyberpratibha.com/blog/fcrackzip-how-to-crack-zip-password/)
2. JohnTheRipper (zip2john, rar2john)

```
zip2john [FILE].zip > [HASH FILE NAME]   # Creating the hash
john [HASH FILE NAME] --wordlist [ROCKYOU.TXT]   # Bruteforce
```

## Network

1. [tshark](https://www.wireshark.org/docs/man-pages/tshark.html)

```
tshark -F pcap -r [FILE].pcapng -w [FILE].pcap
```
