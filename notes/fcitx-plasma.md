The KDE plsama desktop in Ubuntu 18.04 (KDE ver 5.17) has a few bugs that make the use of [fcitx](https://fcitx-im.org) (or other input methods) difficult.

* `Meta+Space` defaults to the KRunner bar and cannot be removed, conflicting with input mehtod: See [this bug](https://bugs.kde.org/show_bug.cgi?id=413542). 
    
    Basically, edit the KRunner shortcut to remove the key combination. Also edit `$HOME/.config/kglobalshortcutsrc` and remove all occurrences of `Meta+Space`.
    
* Plasma empties the `QT_IM_MODULE` variable on start: 

    `echo 'export QT_IM_MODULE=fcitx' > $HOME/.config/plasma-workspace/env/fcitx.sh`.

* The issues are gone in Ubuntu 20.04.
