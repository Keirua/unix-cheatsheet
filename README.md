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

## xargs

    echo a b |xargs -n1 -I plop echo plop

## watch

### Periodically execute a command

    watch -n 1 -t date