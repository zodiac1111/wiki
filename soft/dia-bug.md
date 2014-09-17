# dia错误

来源:https://bugs.launchpad.net/ubuntu/+source/dia/+bug/1102960 7楼

出现类似如下错误,之后崩溃:

```text
** (dia-gnome:11633): CRITICAL **: dia_renderer_set_size: assertion `irenderer != NULL' failed
** (dia-gnome:11633): CRITICAL **: dia_renderer_set_size: assertion `irenderer != NULL' failed
** (dia-gnome:11633): CRITICAL **: dia_renderer_set_size: assertion `irenderer != NULL' failed
Segmentation fault
```

暂时的解决方法:

这个文件 `$(HOME)/.dia/persistence` :

```xml
  <dia:boolean role="view_antialised">   //<- 反锯齿
    <dia:attribute name="booleanvalue">
      <dia:boolean val="true"/>
    </dia:attribute>
  </dia:boolean>
```

i.e. make the antialiased rendering default. And please don't use the
View/AntiAliased menu item to toggle the renderer later, because that would
trigger the crash again.

但是不能在点击 查看->反锯齿,否则还会崩溃

## 默认对齐到网格

找到`$(HOME)/.dia/persistence`配置文件中

```xml
<dia:boolean role="grid_snap">
  <dia:attribute name="booleanvalue">
    <dia:boolean val="true"/>
  </dia:attribute>
</dia:boolean>
```

将`grid_snap`项的值修改为`ture`.默认对齐到网格.