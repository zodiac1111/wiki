# gnome2一样的窗口操作

来源 :http://unix.stackexchange.com/questions/28514/how-to-get-altright-mouse-to-resize-windows-again

```
gsettings set org.gnome.desktop.wm.preferences resize-with-right-button true
gsettings set org.gnome.desktop.wm.preferences mouse-button-modifier '<Alt>'
```