# tar

> * http://www.noah.org/wiki/tar_notes
> * http://pjack1981.blogspot.com/2012/04/tar-with-uid-and-guid.html

打包时设置解包后文件属于的用户和组 
```
tar --numeric-owner --owner=0 --group=0 -cz -f install_tree.tar.gz ${STAGING_DIR}
```

`-h` 包含符号连接,不转储动态链接，转储动态链接指向的文件。 **符号连接变真实文件** 

-T 使用文件列表创建