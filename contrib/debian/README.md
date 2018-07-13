
Debian
====================
This directory contains files used to package Aquilad/Aquila-qt
for Debian-based Linux systems. If you compile Aquilad/Aquila-qt yourself, there are some useful files here.

## Aquila: URI support ##


Aquila-qt.desktop  (Gnome / Open Desktop)
To install:

	sudo desktop-file-install Aquila-qt.desktop
	sudo update-desktop-database

If you build yourself, you will either need to modify the paths in
the .desktop file or copy or symlink your Aquilaqt binary to `/usr/bin`
and the `../../share/pixmaps/Aquila128.png` to `/usr/share/pixmaps`

Aquila-qt.protocol (KDE)

