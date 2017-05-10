# m4b.py

This is a simple python script to convert and split m4b audio books into mp3 files.


## Requirements

* [Python](http://www.python.org/download/) (tested with v2.7)
* [ffmpeg](https://ffmpeg.org/) (tested with v3.3)
* [mp4v2](https://code.google.com/archive/p/mp4v2/downloads) (tested with v2.0.0)


## Installation

### Windows

1. Install python 2.7.xx (https://www.python.org/downloads/windows/)
2. Download [ffmpeg](http://ffmpeg.zeranoe.com/builds/) and place `ffmpeg.exe` in this directory or your `PATH`.
3. Download [libmp4v2.dll](https://github.com/valekhz/libmp4v2-dll/zipball/v0.1) or [compile](http://code.google.com/p/mp4v2/wiki/BuildSource) your
own dll, then place it in this directory.

### Ubuntu 10.10

1. Install packages: `sudo apt-get install python2.7 ffmpeg libavcodec-extra-52`
2. Download mp4v2 then [compile](http://code.google.com/p/mp4v2/wiki/BuildSource) and install.

### Ubuntu 12, 14, 16

1. Install packages: `sudo apt-get install python ffmpeg libavcodec-extra libmp4v2-2`

### Debian 9.0 Stretch - 2017

1. Install packages: `sudo apt-get install python ffmpeg libavcodec-extra libmp4v2-2`

## Usage

There are two ways to use this script:

1. Drag your `.m4b` file(s) onto `m4b.py`.
2. Using the command line which also offers more advanced options.


### Command Line Help

	usage: m4b.py [-h] [-o DIR] [--custom-name "STR"] [--ffmpeg BIN]
				  [--encoder BIN] [--encode-opts "STR"] [--ext EXT] [--pipe-wav]
				  [--skip-encoding] [--no-mp4v2] [--debug] [--keep-tmp-files]
				  [--not-audiobook] [-b BITRATE] [-s SAMPLERATE]
				  [--extract-cover-art]
				  filename [filename ...]

	Split m4b audio book by chapters.

	positional arguments:
	  filename              m4b file(s) to be converted

	optional arguments:
	  -h, --help            show this help message and exit
	  -o DIR, --output-dir DIR
							directory to store encoded files
	  --custom-name "STR"   customize chapter filenames (see README)
	  --ffmpeg BIN          path to ffmpeg binary
	  --encoder BIN         path to encoder binary (default: ffmpeg)
	  --encode-opts "STR"   custom encoding string (see README)
	  --ext EXT             extension of encoded files
	  --pipe-wav            pipe wav to encoder
	  --skip-encoding       do not encode audio (keep as .mp4)
	  --no-mp4v2            use ffmpeg to retrieve chapters (not recommended)
	  --debug               output debug messages and save to log file, also keeps
							tmp files
	  --keep-tmp-files      keep temporary files
	  --not-audiobook       do not add genre=Audiobook
	  -b BITRATE, --bitrate BITRATE
							bitrate for mp3 encoding, integer (example 64)
	  -s SAMPLERATE, --samplerate SAMPLERATE
							sample Rate for mp3 encoding (example 22050
	  --extract-cover-art   extracts cover art as cover.jpg


#### Chapter filenames

You can customize the chapter filenames with `--custom-name "STR"` where `STR` is a valid python [format string](http://docs.python.org/library/stdtypes.html#string-formatting-operations).

Default: three digit chapter number, with leading zeros, and chapter title ("003 - My Title.mp3"):

    --custom-name "%(num)03d - %(title)s"

Chapter title ("My Title.mp3"):

    --custom-name "%(title)s"

Chapter number, without leading zeros, and chapter title  ("3 - My Title.mp3"):

    --custom-name "%(num)d - %(title)s"


#### Encoding

By default the audio will be encoded with the lame mp3 codec using [ffmpeg](http://www.ffmpeg.org/ffmpeg-doc.html). The bit rate and sampling freq will be the same as the source file.
If you wish to use other settings you can specify your own encoding options with `--encode-opts "STR"`. `STR` will be passed to the encoder (`--encoder` or skip to use ffmpeg). Variables available:

    %(outfile)s - output filename (required)
    %(infile)s - .m4b file
    %(bit_rate)d - bit rate of .m4b file
    %(sample_rate)d - sampling rate of .m4b file


### Examples

Convert multiple audio books (you can also drag multiple m4b files onto `m4b.py`):

    python m4b.py myfile.m4b otherfile.m4b

Include chapter number in the generated filenames: (example: "Chapter 10 - Some Title.mp3")

    python m4b.py --custom-name "Chapter %(num)d - %(title)s" myfile.m4b

If you rather want .mp4 files you can skip encoding to speed up the conversion process:

    python m4b.py --skip-encoding myfile.m4b

Force sampling freq to 22050 Hz and bit rate to 128 kbit/s:

    python m4b.py --bitrate 128 --samplerate 22050 myfile.m4b

Encode with lame.exe:

    python m4b.py --encoder lame.exe --pipe-wav --encode-opts "-V 3 -h - %(outfile)s" myfile.m4b

## Bugs

* Does not add cover art to chapters other than 1

## Change Log

* 4/2017 - 
	* Fix encoder error: Too many packets buffered for output stream 0:1.
	* Updated some installation instructions
	* Quiet output of ffmpeg durring encoding, ~~(except in debug)~~
	* Improved filenaming, *'%(num)03d \- %(title)s'*, with some inteligents
	* Added ID3 track numbers
	* Added ID3 genre=Audiobook (by default)
	* Delete temp files/folder (by default)
	* Automaticly changes bitrate 63kbps(M4B) to 64kbps(MP3), increase compatablity
* 5/2017-
	* Fixed problem with encoding file with single chapter
	* Extracts cover art, as command line option (works most of the time)
	* bug fixes

## TODO
* Check for chapers of zero time
* Cover art is missing on tracks!=1
* Check compatability for windows, etc
* Change default output dir to be current working directory
* Fix extract_cover_art, does not work all the time
