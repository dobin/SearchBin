# SearchBin

SearchBin is a fast commandline program for searching within binary files. It's a bit like grep for binaries.

It has three capabilities for searching.
* Search for bytes using hexidecimal `-p`
* Search for a smaller binary file `-f`
* Search for a plain text string `--text`

This is based on [Sepero/SearchBin](https://github.com/Sepero/SearchBin) last updated 2014.


## Examples

Search for the hex bytes "FF14DE" in the file gamefile.db:

```
$ python3 searchbin.py --text "ELF" /bin/bash
offset:              1  0x1             /bin/bash
00000001  45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 03   ELF.............
00000011  00 3e 00 01 00 00 00 f0 2e 03 00 00 00 00 00 40   .>.............@
```

You can also search for unknown patterns with "??". Just insert them where ever you have an unknown byte:
```
$ python3 searchbin.py -p "454c??02" /bin/bash
offset:              1  0x1             /bin/bash
00000001  45 4c 46 02 01 01 00 00 00 00 00 00 00 00 00 03   ELF.............
00000011  00 3e 00 01 00 00 00 f0 2e 03 00 00 00 00 00 40   .>.............@
```


## Options

```
$ ./searchbin.py --help

Optional Arguments:
  -h, --help            show help message and exit
  -f FILE, --file FILE  file containing patterns to search for
  -t PATTERN --text PATTERN 
                        text string like "AB"
  -p PATTERN, --pattern PATTERN
                        hex string like "4142" or "0x41 0x42"
  -b NUM, --before NUM  print this many bytes before start of match
  -a NUM, --after NUM   print that many bytes after start of match
  --buffer-size NUM     read buffer size (in bytes). 8MB default
  -s NUM, --start NUM   starting position in file to begin searching, as bytes
  -e NUM, --end NUM     end search at this position, measuring from beginning
                        of file
  --max-matches NUM     maximum number of matches to find (0=infinite)
  -l FILE, --log FILE   write matched offsets to FILE, instead of standard
                        output
  -v, --verbose         verbose, output the number of bytes searched after
                        each buffer read
  -V, --version         print version information
  -d, --debug           debugging (don't use this)
```


Extra Notes:
An argument --text or -p or -f is required. The -p argument accepts a 
hexidecimal pattern string and allows for missing characters, 
such as 'FF??FF'. When using -f argument, the pattern file will 
be read as a binary file (not hex strings). If no search files are 
specified, %prog will read from standard input. The minimum memory 
required is about 3 times the size of the pattern byte length. 
Increasing buffer-size will increase program search speed for 
large search files. All size arguments (--buffer-size -s -e) are read in decimal 
format, for example: '-s 1024' will start searching after 1kilobyte.
Pattern files do not allow for wildcard matching.
Reported matches are displayed as 0-based offset.



Further Examples:
Search for the text string "Tom" in myfile.exe. Text is case sensitive.
./searchbin.py --text "Tom" myfile.exe


Search for the text string "T?m" in myfile.exe, where ? is a wildcard. This will match "Tom" "Tim" "Twm" and all other variations, including non-printing bytes.
./searchbin.py --text "T?m" myfile.exe


Search for the hexidecimal pattern "AABBCCDDEE" in myfile.exe.
./searchbin.py -p "AABBCCDDEE" myfile.exe


Searches for the hexidecimal pattern "AA??CC??EE" in myfile.exe, where ?? can be any byte value.
./searchbin.py -p "AA??CC??EE" myfile.exe


Takes the binary file pattern.bin, and searches for an exact match within myfile.exe.
./searchbin.py -f pattern.bin myfile.exe


Control output format by showing bytes before and after matches:
./searchbin.py -p "AABB" --before 8 --after 16 myfile.exe


# Original attribution

Please report bugs & feature requests to  sepero 111 @ gmx . com
  or https://github.com/Sepero/SearchBin/issues
  or http://seperohacker.blogspot.com/2012/04/binary-grep-program-searchbin.html


NOTE:
This program is no longer being maintained. I attempted to make the code easily readable and well documented. Please fork it and make something even greater!
