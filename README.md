# Virtual Box

## settings
* Install "VirtualBox Guest Additions" from "Insert Guest Additions CD image" in "Device" on menu bar
* System -> Motherboard -> Base Memory
* System -> Motherboard -> Chipset = ICH9
* System -> Motherboard -> Enable I/O APIC
* System -> Processor -> Extended Features -> Enable PAE/NX
* System -> Acceleration -> KVM
* Display -> Screen -> Enable 3Dd Acceleration

## setting files in ubuntu

```script:/etc/rc.local
PATH=/sbin:/bin:/usr/sbin:/usr/bin
echo "/etc/rc.local"
xkbcomp -I$HOME/.xkb ~/.xkb/keymap/mykbd $DISPLAY 2> /dev/null
mount -t vboxsf Ubuntu /mnt/shared/
```

```script:/etc/profile.d/mysetup.sh(new_file)
#!/bin/sh
echo "/etc/profile.d/mysetup.sh"
xkbcomp -I$HOME/.xkb ~/.xkb/keymap/mykbd $DISPLAY 2> /dev/null
```

## shared folder
* create folde in windows  
* vbox->settings->shared folders->machine folders->add (auto mount, permanent)  
* create directory in ubuntu  
%> sudo mkdir /mnt/shared  
%> sudo chmod 777 /mnt/shared/  
* auto mount. add the following into /etc/rc.local  
> mount -t vboxsf Ubuntu /mnt/shared  

# Ubuntu

## Background color
3476C2

## workspaces
System settings -> Appearance -> Behaviors -> enable workspaces

## Hot keys (Caps to Cursor)
* Change key bindings

%> mkdir ~/.xkb/keymap/  
%> mkdir ~/.xkb/symbols/  
%> subl ~/.xkb/symbols/mykbd &  
```mykbd
xkb_keymap {
    xkb_keycodes  { include "evdev+aliases(qwerty)" };
    xkb_types     { include "complete"  };
    xkb_compat    { include "complete"  };
    xkb_symbols   { include "pc+us+inet(evdev)+myswap(swapkeys)" };
    xkb_geometry  { include "pc(pc105)" };
};
```
%> subl ~/.xkb/symbols/myswap &  
```myswap
partial modifier_keys
xkb_symbols "swapkeys" {
  replace key <CAPS> {[ Meta_L ] };
};
```
%> subl /usr/share/X11/xkb/symbols/pc &  
```pc
    key <ALT>  {	[ NoSymbol, Alt_L	]	};
    include "altwin(meta_alt)"
    //modifier_map Mod3   { <ALT> };

    key <META> {	[ NoSymbol, Meta_L	]	};
    //modifier_map Mod1   { <META> };
    modifier_map Mod2   { <META> };
```

%> subl /usr/share/X11/xkb/symbols/altwin &  
```altwin
partial modifier_keys 
xkb_symbols "meta_alt" {
    //key <LALT> { [ Alt_L, Meta_L ] };    -->
    key <LALT> { [ Alt_L ] };
    key <RALT> { type[Group1] = "TWO_LEVEL",
                 symbols[Group1] = [ Alt_R, Meta_R ] };
    //modifier_map Mod1 { Alt_L, Alt_R, Meta_L, Meta_R }; -->
    modifier_map Mod1 { Alt_L };
//  modifier_map Mod4 {};
};
```

* load the setting
%> subl .mystartup &  
``` .mystartup
#xkbcomp -I$HOME/.xkb ~/.xkb/keymap/mykbd $DISPLAY 2> /dev/null
```
chmod a+x .mystartup

register into gnome-session-properties


* set hot keys using Autokey
 - install Autokey using software center
 - set hotkeys like the following:
    keyboard.send_key("<left>")
-  register into gnome-session-properties

## Caps to Ctrl
%> subl /etc/default/keyboard  
XKBOPTIONS="ctrl:nocaps"  

## Japanese
System settings->Language Support->Install Japanese  
Text Entry setting -> Add (Japanese(Anthy))  

## .bashrc
%> subl .bashrc
else
    #PS1='${debian_chroot:+($debian_chroot)}\u@\h:\w\$ '
    PS1='${debian_chroot:+($debian_chroot)}\w\$ '
fi


# tools

## astah

## Sublime text
https://packagecontrol.io/installation  
Ctrl+Shift+p -> package controll:install package  
-Flatland
-brackethighlighter
-converttoutf8
-simpleclone
-sublimecodeintel
-terminal
-trailingspaces
-xaml
★設定ファイルコピー

## git, ssh
%> apt-get update  
%> sudo apt-get install git  
%> sudo apt-get install gitk  
%> ssh-keygen  
%> subl .ssh/id_rsa.pub &  
%> git config --global user.name "take"  
%> git config --global user.email take@gmail.com  

## Meld
from software center  
add this into .gitconfig  
[diff]  
	tool = meld  
%> git difftool -d  

## console
%> sudo apt-get install g++  

## php
%> sudo apt-add-repository ppa:ondrej/php5-5.6
%> sudo apt-get update
%> sudo apt-get install php5
