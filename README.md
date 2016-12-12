# Unix cheatsheet

 - ln
 - find
 - grep
 - curl
 - rsync
 - tar
 - watch
 - uname

## ln

    ln -s {filename} {symbolic-filename}

    $ sudo ln -s /usr/bin/python3 /usr/bin/python
    $ ls /usr/bin/python -al
    lrwxrwxrwx 1 root root 16 déc.   6 10:34 /usr/bin/python -> /usr/bin/python3

## find

### Find by filename

    $ find . -name "main.c"

### Find by filename with regex

    $ find . -name "tar"
    # no results
    $ find . -iname "*tar" # quotes matter
    ./understanding-tar

### Find only files or directories

Find all directories

    $ find . -type d

Find only the normal files

    $ find . -type f

### Limit search depth

    $ find . -maxdepth 2 -type d

### Find recently accessed or modified files

Find all the files modified less than 60 minutes ago :

    $ find . -mmin -60

Find all the files accessed less than 60 minutes ago :

    $ find -amin -60


## grep

### Search a string/regex inside a file

    $ grep "string_or_regex" filename

By default, the search is case sensitive. It can be **case insensitive** using **grep -i**

    $ grep -i "is" filename

-> It will match "is", "Is", "iS" or "IS"

### **Whole word search** requires the **-w** flag

    $ grep -w "it" filename

-> "it" will match, but not "git"

### Display the 3 lines around the match :

    $ grep -C 3 "string" FILENAME

### Invert match using grep -v

    $ grep -v "git" README.md

Display all the lines in the file README that do not contain "git"

### Display matching line requires the -n flag

    $ grep -n "git" README.md
    66:-> "it" will match, but not "git"

git is on line 66

## curl

### Follow redirects : 

    curl -I -L  www.google.com
    HTTP/1.1 302 Found
    Cache-Control: private
    Content-Type: text/html; charset=UTF-8
    Location: http://www.google.fr/?gfe_rd=cr&ei=lgdIWKvcNoLf8gfD0regAQ
    Content-Length: 258
    Date: Wed, 07 Dec 2016 12:59:02 GMT

    HTTP/1.1 200 OK
    ...

### Display headers :

#### With a HEAD method :

    $ curl -I www.google.com
    HTTP/1.1 302 Found
    Cache-Control: private
    Content-Type: text/html; charset=UTF-8
    Location: http://www.google.fr/?gfe_rd=cr&ei=kAdIWIDIEozf8gfDkbGoDA
    Content-Length: 258
    Date: Wed, 07 Dec 2016 12:58:56 GMT

#### But most of the time :

After a regular request (for instance a POST), use the -D- option to display the response headers

    curl -X POST -H "Authorization: Basic anVubzpqdW5v" -H-Control: no-cache" -d '{
        "aboId":"69401386",
        "sourceUrl":"http://www.elfenmag.tn/wp-content/uploads/2015/11/portrait.jpg"
    }' "http://ccamin.api-private.dev/pictures/matchmaking/members/69401386/pictures/album/" -D-

### Download a file with it's original file name

    curl -O "https://artifacts.someproject.net/artifactory/some-snapshot/net/main.tar"

### Download and rename a file from url

    curl -o artifact.tar "https://artifacts.someproject.net/artifactory/some-snapshot/net/main.tar"

## rsync

### Download a distant file to the local machine

Le fichier est sur un serveur, distant on souhaite le télécharger dans un répertoire local

    rsync -schavzP --stats 'user@remotehost:/data/dump.sql' .

## ssh

### Provice a local public key to a server :

    $ cat ~/.ssh/nom_de_cle.pub | ssh user@serveur.com 'cat - >> ~/.ssh/authorized_keys'

### Creating a SSH tunnel

Opens a tunnel from the local port 5432 on server.com to the local port 6668

    ssh -f -N -T -L 6668:127.0.0.1:5432 user@server.com

## tar

Because I got sick of being [like this](https://xkcd.com/1168/).

### Creating an uncompressed archive using tar command

    $ tar cvf archive_name.tar dirname/

    **c** : create a new archive
    **v** : verbosely list files which are processed.
    **f** : following is the archive file name

Uncompressed archives can be updated with the **r flag** : files/directories can be added : 

    $ tar rvf archive_name.tar newFile
    $ tar rvf archive_name.tar newDirectory/

The new file or new directory will be added to the existing archive_name.tar.

    $ tar rvf archive_name.tar newdir/

### Creating a compressed archive

There are 2 options for compression :

 - gzip using the **z flag**
    
    $ tar cvzf archive_name.tar.gz dirname/

 - bz2 using the **j flag**

    $ tar cvjf archive_name.bz2 dirname/


### Extracting all the files

Extraction require the use of the **-x** flag. The exact command depends on the type of compression used, if any.

 - The archive is not compressed :

    $ tar xvf archive_name.tar

 - The archive is compressed using gzip, use the **z** flag :

    $ tar xvzf archive_name.tar.gz

 - The archive is compressed using bz2, use the **j** flag :

    $ tar xvjf archive_name.tar.gz

### Extract only one/many files or directories

Simply specify which file or directory you want to extract

    $ tar xvf archive_file.tar /path/to/file/or/dir/

With many file or directories, it's the same : simply specify a list.

    $ tar xvf archive_file.tar /path/to/file/or/dir1/ /path/to/file/or/dir2/

You can also specify a pattern using a regex with the flag --wildcards :

    $ tar xvf archive_file.tar --wildcards '*.py'

## watch

### Periodically execute a command

Every 1 second, execute the "date" command

    $ watch -n 1 -t date


## uname

uname give some sytem details information. The -a (or --all) option prints everything.

    $ uname -a
    Linux pc-keirua 4.4.0-51-generic #72-Ubuntu SMP Thu Nov 24 18:29:54 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux

`man uname` gives some other options in order to get information with a lower granularity level

    $ uname -s
    Linux
    $ uname -n
    pc-work
    $ uname -m 
    x86_64

    