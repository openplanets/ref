usage: fido.py [-h] [-v] [-q] [-recurse] [-zip] [-input INPUT]
               [-useformats INCLUDEPUIDS] [-nouseformats EXCLUDEPUIDS]
               [-matchprintf FORMATSTRING]
               [-nomatchprintf FORMATSTRING] [-bufsize BUFSIZE] [-show SHOW]
               [-loadformats XML1,...,XMLn] [-confdir CONFDIR] [-checkformats]
               [-convert] [-source SOURCE] [-target TARGET]
               [FILE [FILE ...]]

Format Identification for Digital Objects (fido). FIDO is a command-line tool
to identify the file formats of digital objects. It is designed for simple
integration into automated work-flows.

positional arguments:
  FILE                  files to check. If the file is -, then read content
                        from stdin. In this case, python must be invoked with
                        -u or it may convert the line terminators.

optional arguments:
  -h, --help            show this help message and exit
  -v                    show version information
  -q                    run (more) quietly
  -recurse              recurse into subdirectories
  -zip                  recurse into zip and tar files
  -input INPUT          file containing a list of files to check, one per
                        line. - means stdin
  -useformats INCLUDEPUIDS
                        comma separated string of formats to use in
                        identification
  -nouseformats EXCLUDEPUIDS
                        comma separated string of formats not to use in
                        identification
  -matchprintf FORMATSTRING
                        format string (Python style) to use on match. See
                        nomatchprintf, README.txt.
  -nomatchprintf FORMATSTRING
                        format string (Python style) to use if no match. See
                        README.txt
  -bufsize BUFSIZE      size of the buffer to match against
  -show SHOW            show "format" or "defaults"
  -loadformats XML1,...,XMLn
                        comma separated string of XML format files to add.
  -confdir CONFDIR      configuration directory to load_fido_xml, for example,
                        the format specifications from.
  -checkformats         Check the supplied format XML files for quality.

Open Planets Foundation (http://www.openplanetsfoundation.org)
See License.txt for license information.
Download from: http://github.com/openplanets/fido/downloads
Author: Adam Farquhar, 2010
Maintainer: Maurice de Rooij, 2011
FIDO uses the UK National Archives (TNA) PRONOM File Format descriptions. PRONOM is available from www.tna.gov.uk/pronom.

Installation
------------

Any platform
1. Download the latest zip release from http://github.com/openplanets/fido/downloads
   (or use the big Downloads button on http://github.com/openplanets/fido)
2. Unzip into some directory
3. Open a command shell, cd to the directory that you placed the zip contents into and cd into folder 'fido'
4. You should now be able to see the help text: 
   python fido.py -h

Dependencies
------------

Fido 0.9.6 and later will run on Python 2.6 or Python 2.7 with no other dependencies.

Format Definitions
------------------

By default, Fido loads format information from two files conf/formats.xml
and conf/format_extensions.xml. Addition format files can be specified using
the -loadformats command line argument.  They should use the same syntax as 
conf/format_extensions.xml. If more than one format file needs to be specified,
then they should be comma separated as with the -formats argument.

Output
------

Output is controlled with the two parameters matchprintf and nomatchprintf.
Each is a string that may contain formating information.  They have access to
an object called info with the following fields:

printmatch: info.version (file format version X), info.alias (format also called X), info.apple_uti (Apple Uniform Type Identifier), info.group_size and info.group_index (if a file has multiple (tentative) hits), info.count (file N)

printnomatch: info.count (file N)

The defaults for Fido 0.9.6 are:
  printmatch: 
    "OK,%(info.time)s,%(info.puid)s,%(info.formatname)s,%(info.signaturename)s,%(info.filesize)s,\"%(info.filename)s\",\"%(info.mimetype)s\",\"%(info.matchtype)s\"\n"

  printnomatch:
    "KO,%(info.time)s,,,,%(info.filesize)s,\"%(info.filename)s\",,\"%(info.matchtype)s\"\n"

It can be useful to provide an empty string for either, for example to ignore all failed matches, or all successful ones (see examples below). 
Note that a newline needs to be added to the end of the string using \n.

Examples
--------

Identify all files in the current directory and below, sending output
into file-info.csv
   python fido.py -recurse . > file-info.csv

Do the same as above, but also look inside of zip or tar files:
   python fido.py -recurse -zip . > file-info.csv

Take input from a list of files:
Linux:
   ls > files.txt
   python fido.py -input files.txt
Windows:
   dir /b > files.txt
   python fido.py -input files.txt

Take input from a pipe:
Linux:
   find . -type f | python fido.py -input -
Windows:
   dir /b | python fido.py -input -

Only show files that could not be identified.
   python fido.py -matchprintf "" .

Only show files that could be identified.
   python fido.py -nomatchprintf "" .

License information
-------------------

See the file "LICENSE.txt" for information on the history of this
software, terms & conditions for usage, and a DISCLAIMER OF ALL
WARRANTIES...
