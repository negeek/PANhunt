PANhunt
========

## Introduction

PANhunt is a tool that can be used to search drives for credit card numbers (PANs). This is useful for checking PCI DSS scope accuracy. It's designed to be a simple, standalone tool that can be run from a USB stick. PANhunt includes a python PST file parser.

## Build

PANhunt is a Python script that can be easily converted to a standalone Windows executable using PyInstaller.

panhunt.py requires:

	- Python 2.7
	- Colorama (https://pypi.python.org/pypi/colorama)
	- Progressbar (https://pypi.python.org/pypi/progressbar)
	- PyInstaller (https://pypi.python.org/pypi/PyInstaller)

To create panhunt.exe as a standalone executable with an icon run:

```
pyinstaller.exe panhunt.py -F
```

You will find the Windows panhunt.exe (built on Windows using pyinstaller) and the Linux panhunt binary here (built on CentOS 6 using pyinstaller).


##Usage
#### For Linux and Windows

```
usage: panhunt [-h] [-s SEARCH] [-x EXCLUDE] [-t TEXTFILES] [-z ZIPFILES] [-e SPECIALFILES] [-m MAILFILES] [-l OTHERFILES] [-o OUTFILE] [-u]

PAN Hunt v1.1: search directories and sub directories for documents containing PANs.

optional arguments:
  -h, --help       show this help message and exit
  -s SEARCH        base directory to search in (default: C:\)
  -x EXCLUDE       directories to exclude from the search (default: C:\Windows,C:\Program Files,C:\Program Files (x86))
  -t TEXTFILES     text file extensions to search (default: .doc,.xls,.xml,.txt,.csv)
  -z ZIPFILES      zip file extensions to search (default: .docx,.xlsx,.zip)
  -e SPECIALFILES  special file extensions to search (default: .msg)
  -m MAILFILES     email file extensions to search (default: .pst)
  -l OTHERFILES    other file extensions to list (default: .ost,.accdb,.mdb)
  -o OUTFILE       output file name for PAN report (default: panhunt_YYYY-MM-DD-HHMMSS.txt)
  -C CONFIG        configuration file to use
  -X EXCLUDEPAN    the PAN to exclude from search
  -u               unmask PANs in output (default: False)
```

Simply running it with no arguments will search the C:\ drive for documents containing PANs, and output to panhunt_<timestamp>.txt.

## Example Output

```
Doc Hunt: 100% ||||||||||||||||||||||||||||||||||||||||| Time: 0:00:01 Docs:299
PAN Hunt: 100% |||||||||||||||||||||||||||||||||||||||||| Time: 0:00:02 PANs:99
FOUND PANs: D:\lab\Archive Test Cards.zip (21KB 19/02/2014)
        Archived Test Cards Excel 97-2003.xls AMEX:***********0005
        Archived Test Cards Excel 97-2003.xls AMEX:***********8431
		...
FOUND PANs: D:\lab\Archived Test Cards Word 2010.docx (19KB 18/02/2014)
        word/document.xml Visa:************1111
        word/document.xml Visa:************1881
        word/document.xml Visa:************1111
        word/document.xml Visa:************0000
		...
FOUND PANs: D:\lab\test card text file.txt (47B 26/02/2014)
         Visa:************1111
         Visa:****-****-****-1111
		...
Report written to panhunt_YYYY-MM-DD-HHMMSS.txt
```
#### For Mac and Linux
Get the adamcaudill-ccsrch-292a1a8 file and unzip. change directory to the folder.
There is an executable file named ccrsh
##### Usage
```
Usage: ./ccsrch [options] [start path]
	
  where [options] are:
    -b             Add the byte offset into the file of the number
    -e             Include the Modify Access and Create times in terms
                   of seconds since the epoch
    -f             Just output the filename with potential PAN data
    -j             Include the Modify Access and Create times in terms
                   of normal date/time
    -o [filename]  Output the data to the file [filename] vs. standard out
    -t [1 or 2]    Check if the pattern follows either a Track 1
                   or 2 format
    -T             Check for both Track 1 and Track 2 patterns
    -c             Show a count of hits per file (only when using -o)
    -s             Show live status information (only when using -o)
    -l N           Limits the number of results from a single file before going
                   on to the next file.
    -n [list]      File extensions to exclude (i.e .dll,.exe)
    -h 
```
## Function

The script uses regular expressions to look for Visa, MasterCard or AMEX credit card numbers in document files. Zip files are recursed to look for document files. PST and MSG files are parsed and emails and attachments searched in. The script will list but does not yet search Access databases.

## Configuration

The script allows for a configuration to be written that will default the application with settings such that you don't need to
repeatedly specify exclude/include paths or the test PANs to exclude.
