# puckman

Installs software directly from github repos. I wanted an approach to what Arch and AUR provide: cutting edge versions of software fresh from the repo oven. Thus the name `puckman`, which is the initial original name of the pacman character from the videogame (since it has the shape of a hockey puck). 

It smartly identifies the appropriate installer from the list of release artifacts. Don't get carried away, this intelligent detection is just a buch of 'if's

It is possible to define a regex to make sure the right package is selected. People do sometimes use strange names and formats for the release packages. 

It is optimized for debian/ubuntu systems. 

Warning: it doesn't have any kind of journal or history. It can't uninstall. This is not a big issue for deb files but for single binary installations you have to manually remove the file from /usr/local/bin and fish the config files scattered through your file system. 

