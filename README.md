# Unix cheatsheet

Some CLI goodness. A good place to get started before reading this collection of tools is [The Art Of Command Line](is https://github.com/jlevy/the-art-of-command-line).

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

### Display only the match, not the whole line :

use option -o, or --only-matching

    $ grep "th" *

    some-text-file : the cat sat on the mat  
    some-other-text-file : the quick brown fox  
    yet-another-text-file : i hope this explains it thoroughly 

    $ grep -o "\w*th\w*" *

    the
    the
    the
    this
    thoroughly


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

## uname && lsb_release

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

lsb_release also gives details about the distro info

lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 16.04.1 LTS
Release:	16.04
Codename:	xenial

## od

od prints the octal representation of files

    $ od README.md 
    0000000 020043 067125 074151 061440 062550 072141 064163 062545
    0000020 005164 020012 020055 067154 020012 020055 064546 062156
    0000040 020012 020055 071147 070145 020012 020055 072543 066162
    0000060 020012 020055 071562 067171 005143 026440 072040 071141

the -c flag allow the see the printable character

    $ od -c README.md
    0000000   #       U   n   i   x       c   h   e   a   t   s   h   e   e
    0000020   t  \n  \n       -       l   n  \n       -       f   i   n   d
    0000040  \n       -       g   r   e   p  \n       -       c   u   r   l
    0000060  \n       -       r   s   y   n   c  \n       -       t   a   r    

## hd

hd prints the hexadecimal representation of a file

    $ hd README.md 
    00000000  23 20 55 6e 69 78 20 63  68 65 61 74 73 68 65 65  |# Unix cheatshee|
    00000010  74 0a 0a 20 2d 20 6c 6e  0a 20 2d 20 66 69 6e 64  |t.. - ln. - find|
    00000020  0a 20 2d 20 67 72 65 70  0a 20 2d 20 63 75 72 6c  |. - grep. - curl|
    00000030  0a 20 2d 20 72 73 79 6e  63 0a 20 2d 20 74 61 72  |. - rsync. - tar|
    00000040  0a 20 2d 20 77 61 74 63  68 0a 20 2d 20 75 6e 61  |. - watch. - una|
    00000050  6d 65 0a 0a 68 74 74 70  73 3a 2f 2f 67 69 74 68  |me..https://gith|
    00000060  75 62 2e 63 6f 6d 2f 6a  6c 65 76 79 2f 74 68 65  |ub.com/jlevy/the|
    00000070  2d 61 72 74 2d 6f 66 2d  63 6f 6d 6d 61 6e 64 2d  |-art-of-command-|
    00000080  6c 69 6e 65 0a 0a 23 23  20 6c 6e 0a 0a 20 20 20  |line..## ln..   |
    00000090  20 6c 6e 20 2d 73 20 7b  66 69 6c 65 6e 61 6d 65  | ln -s {filename|
    000000a0  7d 20 7b 73 79 6d 62 6f  6c 69 63 2d 66 69 6c 65  |} {symbolic-file|



## nproc

nproc prints the number of processing units available

    $ nproc
    4

## lshw

lshw lists hardware information

    $ lshw

It can output the information as html with the -html flag

    $ lshw

## Display the process tree

With the process names :

    $ pstree

With the process names and PIDs :

    $ pstree -p


## tr

Convert from lowercase to uppercase :

    $ echo "plop" | tr a-z A-Z

Delete repeating characters :

    $ ps aux | tr -s " "

Get only the digiets :

    $ echo "my username is 432234" | tr -cd 0-9
    432234

**-d** removes the digits, and  **-c** gets the complement of the result.
So **tr -cd 0-9** this command means "no (no digits)"


## df

df provides with information about disk space usage

    df -h
    Sys. de fichiers Taille Utilisé Dispo Uti% Monté sur
    udev               3,9G       0  3,9G   0% /dev
    tmpfs              788M    9,8M  778M   2% /run
    /dev/sda2          227G     58G  157G  27% /
    tmpfs              3,9G    7,6M  3,9G   1% /dev/shm
    tmpfs              5,0M    4,0K  5,0M   1% /run/lock
    tmpfs              3,9G       0  3,9G   0% /sys/fs/cgroup
    /dev/sda1          511M    3,6M  508M   1% /boot/efi
    tmpfs              788M     56K  788M   1% /run/user/1000