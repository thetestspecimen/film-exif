# EXIF Data for Film / Analogue Cameras

This repository contains bash scripts to enable the generation of EXIF data relavant to digital images
that have been scanned from negative or positive film.

## Why is this needed?

When using a digital camera or phone, most of the exif data is automatically generated and saved with 
the image. This is obviously not possible with film, so it needs to be added later, after the film has
been scanned into digital format.

You can, of course, use Photoshop, Lightroom, GIMP, etc. to add EXIF data, but it is not typically geared
towards pictures generated from film, and it can sometimes be confusing as to what data should be included.

There are also (currently) no EXIF schemes officially designed for film, so you effectively have to create
your own to cover the missing data. For example, how would you specify the film you used? or the chemicals 
used? or the filter used on the lens? These EXIF fields don't exist in any of the main schemes.

There are of course tools out there to help you do this, but I found that I was having to use too many separate
tools to achieve what is effectively just adding text information to an image.

The aim of these scripts is to speed up the process, and do it all in one place, with just the information
I need.

## What is EXIF data?

Simply put, it is the text stored in a file (in this case an image) that gives information about the images
content, the camera, the lens, the camera settings, the location etc.

This data is covered by various EXIF schemes. I have found that the IPTC generally has the best description 
of the various schemes and their intended uses:

[IPTC EXIF Photo Metadata](https://www.iptc.org/std/photometadata/specification/IPTC-PhotoMetadata)

## What do the scripts do

There are basically two scripts.

The first is intended to be used on a roll of film in batch mode (i.e. all 36 frames from a single roll of 35mm film). This script only contains fields that would be consistent across the whole set of pictures:

1. Camera make, model and serial number
2. Film make, type, development process, development chemicals, ISO
3. Processing lab and scanner used
4. Copyright owner, copyright statement, copyright/terms URL
5. Website and email address

The second file runs through all the fields that are specific to the individual photos on the roll:

1. Title, headline, description
2. Date created
3. Filter used
4. Lens information: Make, model, serial number, max aperture, focal length, aperture used, shutter speed used
5. Location information: Region, city etc. Longitude, latitude, elevation
6. Image number on the film (i.e. "5" if it was the 5th image on the film roll)

## Speeding up the process

Each script file has a section at the top where you can permanently add:

1. Cameras
2. Lenses
3. Films (already filled in with quite a lot)

You can also permanently define copyright and contact/website information so you don't have to type it in each time.

## Installation

As these scripts are written in bash, you should be able to use them without much problem on any unix type system, like Linux 
(Fedora, Ubuntu etc.) and MacOS. Windows is a little more involved but also possible, as you can actually install a native Linux
environment in Windows to get access to a real bash shell.

The desktop I use is Fedora, so I can only confirm a working setup for Fedora. However, I will detail below some instructions
for Ubuntu, Windows and MacOS, but can't guarentee they will work, as I can't easily test them.

### Linux Installation

1. Install exiftool
	- Fedora: `sudo dnf install perl-Image-ExifTool`
	- Ubuntu: `sudo apt install libimage-exiftool-perl`
2. Download ".ExifTool_config" from this repository and put it in your home folder (do not change the filename!), or if you already have 
	a config file then append the contents to your own config file

You are now ready to use the scripts. Go to the next main section for usage details.

### Windows Installation

Note: You must have a 64-bit version of Windows for any of this to work

1. Make sure your windows install is up to date (run windows update)
2. In the start menu open the "Microsoft Store" app
3. Search for Ubuntu. You will see a few options, but you want the one which has the orange logo and only says "Ubuntu"
4. Select "Ubuntu" and click "Install" (Windows will now download and install Ubuntu automatically)
5. You now have Ubuntu installed as an app. Go and open the app from the start menu
6. A commandline will appear and it will say it is installing, let it do its stuff
7. You will then be asked for a Username. Enter whatever you like it doesn't have to be the same as your windows username
8. You will then be asked to type a password. Enter whatever you like (Note: when typing it will not write anything, this is normal.
	just keep going and press enter when finished)
9. Repeat the same password
10. You now have Ubuntu, and more importantly access to bash!
11. Now we must install exiftool. From within ubuntu enter the following command: `sudo apt install libimage-exiftool-perl`
12. Enter the password you used in the setup above when prompted
13. Enter the following command to download the configuration for exiftool: `wget https://github.com/thetestspecimen/film-exif/blob/master/.ExifTool_config`
14. You are now ready to go! Go to the next main section for usage details

### MacOS Installation

Please refer to the official ExifTool installation guide: [Install ExifTool](https://exiftool.org/install.html)

You also need to download the config file (.ExifTool_config) and make sure it is available for ExifTool to use. I have included below some information given on the official ExifTool website to help you acheive just that:

> To activate this file, rename it to ".ExifTool_config" and
> place it in your home directory or the exiftool application
> directory.  (On Mac and some Windows systems this must be done
> via the command line since the GUI's may not allow filenames to
> begin with a dot.  Use the "rename" command in Windows or "mv"
> on the Mac.)  This causes ExifTool to automatically load the
> file when run.  Your home directory is determined by the first
> defined of the following environment variables:
>
> 1.  EXIFTOOL_HOME
> 2.  HOME
> 3.  HOMEDRIVE + HOMEPATH
> 4.  (the current directory)

## Usage

The interface you will see should be the same regardless of the operating system you use, as all will be using bash.

There are two scripts:

1. exif-group
2. exif-ind

You can put the scripts wherever you like. You just need to be sure to open the terminal where the scripts live. This is easy in linux systems as usually
the right click on a mouse will have an option to "open in terminal", which will open the terminal in the directory you are viewing. The other option is to 
"cd" to the correct path.

For windows users:

1. Go to the folder where the scripts are
2. Press "alt" and "shift" together, and then right click on a blank area in the folder
3. One of the options will be "open a linux shell here". Click that.
4. You now have a bash shell open in the folder where the scripts are

There are two ways you can use the scripts.

1. Run the scripts from a consistent place, and specify the full path of the location of the photos
2. Copy the script files to the folder where the photos live, and not have to bother with specifying the full path

You are likely to run into less problems with option 2, but it is completely up to you. I will give examples of both.

### Permissions

One of the problems you may come across is having the incorrect permissions for the scripts and config file.

The config file must be readable. The easiest way to do this is from the commandline:

`sudo chmod +r /home/yourusername/.ExifTool_config`

You will also need to make sure the script files are executable. This can be done on the commandline:

`sudo chmod +x /path/to/script/file/exif-group`

`sudo chmod +x /path/to/script/file/exif-ind`

In linux you could also right click on the file then --> Properties --> Permissions --> Allow executing file as program

### Commands

For the batch command (i.e. to run the script on all files contained within a folder), you can use one of the following:

1. `./exif-group "/home/username/photos-folder/"`
2. `./exif-group .` 

Note: for option 2 to work the scripts and the photos must be in the same folder, otherwise use option 1

For the script which runs on individual photos:

1. `./exif-ind "/home/username/photos-folder/photo.jpg"`
2. `./exif-ind "photo.jpg"`

Note: again, option 2 is only possible if the photo is in the same folder as the script

## Attributions

These scripts would not even exist if it wasn't for the, quiet frankly, unbelievable work done by Phil Harvey to create Exiftool.

If you have time, it would be worth your while to take a look at the program he has created, as it has many features that may be 
useful to you. It is easily the most comprehensive and flexible tool for dealing with EXIF data that is currently available.

[Exiftool Website](https://exiftool.org/)

Phil has also released Exiftool for free! He has begrudgingly added a donations section to his website, so if you have some spare
cash knocking about, I would encourage you to send some his way.

## License

The content of this project is licensed under the [GNU General Public License v3.0](LICENSE.md)
