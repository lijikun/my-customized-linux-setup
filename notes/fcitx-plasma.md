The KDE plsama desktop has a few bugs that make the use of fcitx (or other input methods) difficult.

* `Meta+Space` defaults to the KRunner bar and cannot be removed: See [this bug](https://bugs.kde.org/show_bug.cgi?id=413542). 
    
    Basically, edit the KRunner shortcut to remove the key combination. Also edit `$HOME/.config/kglobalshortcutsrc` and remove all occurrences of `Meta+Space`.
    
* Plasma empties the `QT_IM_MODULE` variable on start: 

    `echo 'export QT_IM_MODULE=fcitx' > $HOME/.config/plasma-workspace/env/fcitx.sh`.
