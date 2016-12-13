Todo

list of the frequent linux cli tools I should detail

http://www.thegeekstuff.com/2010/11/50-linux-commands/

# unix 101

Well detailled there :

http://wiki.linux-france.org/wiki/Les_commandes_fondamentales_de_Linux
http://wiki.linux-france.org/wiki/Les_commandes_fondamentales_de_Linux/Arborescence_Linux

 - man (also apropos, whatis, maybe others)
 - cd (cd directory, cd -, cd)
 - ls -> what do the various options do exactly ?
 - rm
 - cp/mv
 - cat/less/more/head/tail
 - mkdir
 - sudo/su
 - apt-get and similar
 - pwd

# Not 101 but almost

[x] tr
[x] find
[x] grep
[x] tar
[x] ln
 - cut/paste/join
 - sort
 - uniq
 - tee
 - wc (-l, -w, -c)
 - diff/sdiff/cmp
 - expand grep with egrep/rgrep
 - du/df/free
 - watch
 - shutdown
 - sed
 - alias
 - xargs
 - bg/fg
 - screen
 - vim

# more advanced and/or less common tools

 - whatis
 - locate/whereis 
 - bc
 - date/cal
 - chown/chattr/chmod
 - getfacl/setfacl
 - tree
 - stat
 - watch/time/crontab
 - shuf

# system tools

 - last
 - whoami
 - passwd
 - chmod/chown/chattr
 - mount/umount
 - lshw, lsusb
 - uname
 - lsof
 - sysctl
 - journalctl
 - process-related :
     - /proc virtual filesystem
     - ps
     - pstree
     - kill/killall
     - top/htop

# debugging tools

 - strace/ltrace
 - ldd
 - gdb/valgrind
 - file

# network-related tools
 
 - curl/wget
 - rsync
 - ping
 - dig
 - whois
 - host
 - nmap
 - netstat/ss
 - ifconfig
 - traceroute/mtr
 - tcpdump/wireshark (not cli)