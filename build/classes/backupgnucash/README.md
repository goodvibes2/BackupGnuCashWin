############################################################
          BackupGnuCash 1.10 README.md file.

The last known stable series is the 1.1x series.
------------------------------------------------------------

##################
Table of Contents:
------------------

  - Overview
  - Dependencies
  - Invocation/running
  - Internationalization
  - Building & Installing
  - Supported Platforms
  - Getting the Source via SVN          ???
  - Developing GnuCash                  ???

########
Overview
--------

BackupGnuCash is an application for simplifying the creation of
encrypted backups of the data files used by the GnuCash personal
finance manager.
See http://www.gnucash.org for more information about GnuCash.

Free software 7-Zip is used to perform the encryption.

BackupGnuCash project is not part of the GnuCash project.

Features include:

  - Available for both GNU/Linux and Microsoft Windows.
    BackupGnuCash has been tested in GNU/Linux Ubuntu 16.04 and Windows 10.
    GnuCash is also available for Mac OS/X and BackupGnuCash may work
    but this has not (yet) been tested.

  - An easy-to-use interface.
    After finishing a GnuCash session, you just start BackupGnuCash,
    optionally select different directories, enter the password for encryption
    and click the Backup button. BackupGnuCash then encrypts the GnuCash data,
    saved reports and preferences files to a date/time stampded file name in
    the local 3rd party cloud storage directory.

  - All GnuCash data files needed for system recovery are backed up.
    The 3 files backed up into each archive file are:

    1) the main GnuCash data file which usually has a .gnucash extension.
       For example:
       GNU/Linux: /home/[USER_NAME]/GnuCash/[BOOK].gnucash
       Windows:   C:\Users\[USER_NAME]\Documents\GnuCash\[BOOK].gnucash

    2) the saved reports file, for example
       GNU/Linux:   /home/[USER_NAME]/.gnucash/saved-reports-2.4
       Windows: C:\Users\[USER_NAME]\.gnucash\saved-reports-2.4

       Note: the 2.4 suffix is used for both GnuCash 2.4 and 2.6.
         As of 29th May 2016 current stable version of GnuCash is
         2.6.12.

    3) the preferences file, usually
       GNU/Linux:   /home/[USER_NAME]/.gnucash/books/XXXX.gnucash.gcm
       Windows: C:\Users\[USER_NAME]\.gnucash\books\XXXX.gnucash.gcm

    These 3 files are all that is usually required when restoring or
    moving to a new computer, apart from the GnuCash program itself
    which can be downloaded from the GnuCash website.

  - One-time setup of:
    1) the location and name of the GnuCash data file.
       Once a GnuCash data file has been selected in the BackupGnuCash
       window, the file modification date and time is display below it,
       so the user can check it approximately matches the date and time
       of their last GnuCash session.

       If it is found that the GnuCash data file has a file name like
       [BOOK].gnucash.yyyymmddhhmmss.gnucash
       where yyyymmddhhmmss is a string of numbers representing a date
       and time, then probably the user has mistakenly opened a GnuCash
       automatically created backup, and forgotten to re-open the real
       GnuCash data file.

       In this case, to go back to using the intended data file, the user
       should open the latest (or most correct) data file, then use
       'File', 'Save As' to save in the intended data file.
       The next time GnuCash is opened, it will open the intended data 
       file because GnuCash, unless told to open a specific data file,
       will open the last data file it used.

    2) the GnuCash version may optionally be specified which
       will add a suffix of _[Version] to the archive file name
       (before the .7z extension).
       While GnuCash developers try hard to make most versions of
       the data files backwards and forwards compatible, some
       versions are not totally backwards and forwards compatible,
       so this information may be useful if a restore is needed.

    3) the location of the base archive directory, for example:
        GNU/Linux: /home/[USER_NAME]/Dropbox
        Windows:   C:\Users\[USER_NAME]\Dropbox

       The archive is created in a sub-directory of the base archive
       directory, called "GnuCash". For example:
         GNU/Linux: /home/[USER_NAME]/Dropbox/GnuCash
         Windows:   C:\Users\[USER_NAME]\Dropbox\GnuCash

       The GnuCash sub-directory must be manually created. If the GnuCash
       sub-directory of the archive directory does not exist, BackupGnuCash
       will show an error message in the Log area at the bottom of the
       window and the Backup button will be disabled.

    After valid (existing) entry of:
      the location and name of the main GnuCash data file
      the location of the base archive directory
    BackupGnuCash enables the 'Save' Settings' button, which when clicked,
    will save the 2 above entries, and the Version field (optional), in file:
      GNU/Linux: /home/[USER_NAME]/.BupGc/defaultProperties
      Windows: C:\Users\USER_NAME]\.BupGc/defaultProperties

    Note that the password is NOT saved and must be entered each time
    BackupGnuCash is started. The password must be at least 8 characters.

    The next time BackupGnuCash is started, the 3 saved fields will be
    automatically loaded from the defaultProperties file.

    Saving the locations of multiple GnuCash books is currently not
    supported. In this case, if you wish to backup a book which is
    not the one for which the settings are saved, use the Browse button
    to select the required book and optionally select a different
    archive file directory. As the book name is part of the archive
    file name, so long as books have unique names, archive files
    for multiple books may be put in the same archive directory.

  - The archive file is intended to be created in a local directory
    which is replicated in the 'cloud' by a 3rd party cloud storage
    service such as Dropbox, Google Drive or Microsoft OneDrive.
    This makes the backup an off-site and local backup.
    It is the users responsibility to regularly ensure the 3rd party
    cloud storage service is working correctly, say by connecting to the
    service in a web browser or on another computer and checking
    the expected files exist and the contents match the local copy.

    It is also the users responsibility to periodically remove old
    backups from the archive directory to ensure any cloud storage
    capacity limits are not exceeded.

    BackupGnuCash refers to the base archive directory as the "Dropbox"
    directory, but it could be used with any other cloud storage
    service, like Google Drive or Microsoft OneDrive, which operates
    in a similar fashion.

    The archive file name will be created in the following format:
      GnuCash[BOOK]_yyyyMMddhhmmss[_Version].7z
        where
         [BOOK] is the data file name without the .gnucash externsion
         yyyyMMddhhmmss is the current date and time 
         [_Version] is the optional GnuCash version number if entered.

  - Each archive file will contain encrypted copies of the 3 data files.
    The files will be encrypted, using the entered password, by free
    software 7-Zip, using AES-256 encryption.

    BackupGnuCash is not intended to be used to conceal illegal activities.
    The GnuCash data files are encrypted in the archive because, even though
    3rd party cloud storage services usually try hard to ensure the privacy
    of your data, why risk unencrypted data in the cloud if you don't have to?

    BackupGnuCash does not provide any facility for extracting the data files
    from an encrypted archive. 7-Zip must be installed in order for the
    encrypted archives to be created.
    Windows: 7-Zip includes gui tool '7-Zip File Manager' which can be used to
      decrypt the archive files if they need to be restored.
    GNU/Linux, Archive Manager can be used to manage 7-Zip files if the
      p7zip-full package is installed.

    Of course, the user needs to enter the password carefully, and remember it!
    If the password is entered incorrectly or forgotten, the encrypted archives
    will be of no use.

    There is a 'Show' checkbox to the right of the password. If this is checked,
    the password will display. If it is unchecked, the characters of the
    password will display as asterixes.
    After ensuring no-one is watching, show and carefully check the password
    each time you enter it.


???

> I've created a gui (JavaFX/OpenJFX) Java app for backing up GnuCash

Home Page:
None

Precompiled binaries:
http://www.gnucash.org/download



############
Dependencies
------------

The following packages are required to be installed to run GnuCash:

[see README.dependencies]

#######
Running
-------

####################
Internationalization
--------------------

English only

#####################
Building & Installing
---------------------

###################
Supported Platforms
-------------------

GnuCash 2.x is known to work with the following operating systems:

GNU/Linux             -- x86, Sparc, PPC
FreeBSD               -- x86
OpenBSD               -- x86
MacOS X		      -- PPC and Intel, Versions 10.5 and later


GnuCash can probably be made to work on any platform for which Gtk+ can,
given sufficient effort. If you try and encounter difficulty, please join
the developer's mailing list, gnucash-devel@gnucash.org.

#########################
Downloads
-------------------------

GnuCash sources and Mac and Windows binaries are hosted at
SourceForge. Links for the current version are provided at
http://www.gnucash.org. We depend upon distribution packagers for
GNU/Linux and *BSD binaries, so if you want a more recent version than
your distribution provides you'll have to build from source.

##############################
Getting Source with Git
------------------------------

We maintain a mirror of our master repository on Github. You can
browse the code at https://github.com/Gnucash/gnucash. Clone URIs are
on that page, or if you have a Github account you can fork it
there. Note, however, that we do *not* accept Github pull requests:
All patches should be submitted via Bugzilla, see below.

##################
Developing GnuCash
------------------
Before you start developing GnuCash, you should do the following:

1. Read http://wiki.gnucash.org/wiki/Development

2. Look over the doxygen-generated documentation at
    http://code.gnucash.org/docs/HEAD/

3. Go to the GnuCash website and skim the archives of the GnuCash
   development mailing list.

4. Join the GnuCash development mailing list. See the GnuCash website
   for details on how to do this.


Submitting a Patch:

  Patches should be created from a git clone using the appropriate
  branch HEAD. For those unfamiliar with git, instructions on making a
  patch may be found at http://wiki.gnucash.org/wiki/Git#Patches

  Please attach patches to the appropriate bug or enhancement request
  in Bugzilla (https://bugzilla.gnome.org, Project GnuCash). Create a
  new bug if you don't find one that's applicable. Please don't submit
  patches to either of the mailing lists, as they tend to be
  forgotten.


Thank you.
