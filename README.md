# puckman

Installs software directly from github repos. I wanted an approach to what Arch and AUR provide: cutting edge versions of software fresh from the repo oven. Thus the name `puckman`, which is the initial original name of the pacman character from the videogame (since it has the shape of a hockey puck). 

It smartly identifies the appropriate installer from the list of release artifacts. Don't get carried away, this intelligent detection is just a buch of 'if's

It is possible to define a regex to make sure the right package is selected. People do sometimes use strange names and formats for the release packages. 

It is optimized for debian/ubuntu systems.

Install journaling (v3.0+): each successful install writes a small JSON manifest under `PUCKMAN_STATE` (default `/var/lib/puckman/installed/`). Use `puckman uninstall <name>` (or `remove` / `delete`) to reverse installs that were recorded this way: `.deb` packages are purged with `apt-get purge`, single-file/binary installs remove only paths under `/usr/local/bin` that were tracked, and pip lines use the same pip command that was used to install.

Older installs and manual steps are not tracked; config files created at runtime are never removed automatically.
