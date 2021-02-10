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
tar xvf {}
```

## Image
1. [StegSolve](https://github.com/zardus/ctf-tools/blob/master/stegsolve/install)
<pre>
java -jar stegsolve.jar
</pre>

2. Steghide (JPEG, BMP, WAV and AU)
<pre>steghide extract -sf [FILE.JPG]</pre>

3. [Stegcracker](https://github.com/Paradoxis/StegCracker)
<pre>stegcracker [FILE.JPG] [DICTIONARY.TXT]</pre>

4. [Steg_brute](https://github.com/Diefunction/stegbrute)
<pre>python steg_brute.py -b -d [ROCKYOU.TXT] -f [FILE].JPG</pre>

5. [zsteg (PNG)](https://github.com/zed-0xff/zsteg)
<pre>zsteg -a [FILE].png</pre>

6. [zbarimg (QR Code)](http://manpages.ubuntu.com/manpages/bionic/man1/zbarimg.1.html)
<pre>zbarimg [FILE].png/jpg</pre>

## Compressed Files
1. [zcat](https://linux.die.net/man/1/zcat)
<pre>zcat [FILE.tar.gz] > FILE.txt</pre>

2. unzip
<pre>unzip [FILE.ZIP]</pre>

3. 7zip
<pre>7z e [FILE.ZIP]</pre>

4. [fcrackzip](https://www.cyberpratibha.com/blog/fcrackzip-how-to-crack-zip-password/)

5. JohnTheRipper (zip2john, rar2john)
<pre>zip2john [FILE].zip > [HASH FILE NAME]   # Creating the hash
john [HASH FILE NAME] --wordlist [ROCKYOU.TXT]   # Bruteforce</pre>

## Network
1. [tshark](https://www.wireshark.org/docs/man-pages/tshark.html)
<pre>tshark -F pcap -r [FILE].pcapng -w [FILE].pcap</pre>
