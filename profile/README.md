# WineHub Project

Flatpak repo hosting wine as runtime, and apps.  
Idea : prefix is made at build time, user just download and install
```
  Wine (flatpak runtime or baseapp?)
  App and prefix (flatpak app)
```
How to handle different wine versions
- as branches : 
  - i.e org.winehq.wine//vanilla-8.16-23.08, org.winehq.wine//caffe-8.16-23.08

How to handle prefixes?
- at app build time: 
  - what if user changes flatpak runtime with --runtime-version
  - how to make prefix writable with this system, fuse-overlayfs, overlay mount at XDG_DATA_HOME?
- at runtime : ruins the big idea

How to handle windows dependencies?
- at app build time: with winetricks as sdk tool

How to handle user prefix modification
- writable overlay prefix

How to handle package build system?
- winetricks as sdk tool (versioned)
- do not let winetricks download files at buildtime: flatpak-winetricks-generator: to generate list of files to download and put in winetricks cache
- how to handle building a windows app from source?
- add option to allow shared home directory with prefix (xdg-user-dirs)

Filesystem structure
- /app/bin/wineapp : to store wineapp
- /app/share/prefix : to store prefix
- XDG_STATE_HOME/upper : upper writable prefix layer
- XDG_STATE_HOME/work : work layer
- XDG_DATA_HOME/prefix : merged layer

add a launch wrapper that hooks needed env var for app to work correctly and mounting overlayfs
- WINEPREFIX=XDG_DATA_HOME/prefix
- fuseoverlayfs -olower=/app/share/prefix,work=$XDG_STATE_HOME/work,upper=$XDG_STATE_HOME/upper $XDG_DATA_HOME/prefix

food for thought
- handle windows dependencies as runtime extension? how to handle using different versions
- what to use flatpak baseapp system for?
