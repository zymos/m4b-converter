# Convert m4b audio book to mp3

This is a simple python script to convert m4b audio books to a group of mp3
files split by chapter.


## Requirements

* [Python](http://www.python.org/download/) (tested with 2.7)
* ffmpeg
* [mp4v2](http://code.google.com/p/mp4v2/downloads/detail?name=mp4v2-1.9.1.tar.bz2&can=2&q=) (v1.9.1)


## Installation

### Windows

1. Download and install python 2.7.
2. Download [ffmpeg](http://ffmpeg.arrozcru.org/autobuilds/) and place `ffmpeg.exe` in this directory or your `PATH`.
3. Grab [libmp4v2.dll](https://github.com/valekhz/libmp4v2-dll/zipball/v0.1) or [compile](http://code.google.com/p/mp4v2/wiki/BuildSource) your
own dll, then place it in this directory.

### Ubuntu 10.10

1. Install packages: `sudo apt-get install python2.7 ffmpeg libavcodec-extra-52`
2. Download mp4v2 then [compile](http://code.google.com/p/mp4v2/wiki/BuildSource) and install.

## Usage

There are two ways to use this script:

1. Drag your `.m4b` file onto `m4b.py`.
2. Using the command line which also offers more advanced options.


### Command Line Help

    usage: m4b.py [-h] [-o DIR] [--custom-name [STR]] [--ffmpeg-bin EXE]
                  [--encode [STR]] [--ext EXT] [--skip-encoding] [--no-mp4v2]
                  [--debug]
                  <m4b file>
    
    Convert m4b audio book to mp3 file(s).
    
    positional arguments:
      <m4b file>            m4b file to be converted
    
    optional arguments:
      -h, --help            show this help message and exit
      -o DIR, --output-dir DIR
                            directory to store encoded files
      --custom-name [STR]   customize chapter filenames (see README)
      --ffmpeg-bin EXE      path to ffmpeg binary
      --encode [STR]        custom encoding string (see README)
      --ext EXT             extension of encoded files
      --skip-encoding       do not encode audio (keep as .mp4)
      --no-mp4v2            use ffmpeg to retrieve chapters (not recommended)
      --debug               output debug messages and save to m4b.log

#### Chapter filenames

When the chapter number isn't included in the chapter title you may want to include it in the generated chapter filenames.
To do this you can specify `--custom-name 'STR'` where STR is a valid python [format string](http://docs.python.org/library/stdtypes.html#string-formatting-operations). Check the examples below. The
default is `--custom-name '%(title)s'`. Available variables: `num`, `title`, `start`, `end`.

#### Encoding

By default the audio will be encoded with lame mp3, keeping the same bit rate and sampling frequency as the source file.
If you wish to use other settings you can specify your encoding string with `--encode 'STR'` where `STR` is a bunch of
valid ffmpeg encoding parameters. Visit the [ffmpeg docs](http://www.ffmpeg.org/ffmpeg-doc.html) for more info.


### Examples

Include chapter number in the generated filenames: (example: "Chapter 10 - Some Title.mp3")

    python m4b.py --custom-name 'Chapter %(num)d - %(title)s' myfile.m4b

If you rather want .mp4 files you can skip encoding to speed up the conversion process:

    python m4b.py --skip-encoding myfile.m4b

Custom ffmpeg encoding string:

    python m4b.py --encode '-acodec libmp3lame -ar 22050 -ab 128k' myfile.m4b
