# Unix cheatsheet

## ln

    ln -s {filename} {symbolic-filename}

    $ sudo ln -s /usr/bin/python3 /usr/bin/python
    $ ls /usr/bin/python -al
    lrwxrwxrwx 1 root root 16 déc.   6 10:34 /usr/bin/python -> /usr/bin/python3

## find

### Using regex

    $ find . -name "tar"
    # no results
    $ find . -iname "*tar" # quotes matter
    ./understanding-tar

## curl

### Follow redirects : 

    curl -L

### Display headers :

#### With a HEAD method :

    curl -I www.google.com

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


## xargs

    echo a b |xargs -n1 -I plop echo plop

## watch

### Periodically execute a command

    watch -n 1 -t date