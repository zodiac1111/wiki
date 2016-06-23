# mkvtoolnix

导出 mkv 文件内嵌的字幕.

vlc一般没有导出功能.

    mkvtoolnix

软件 `mkvinfo-gui`


Install mkvtoolnix with `sudo apt-get install mkvtoolnix`.

Run from terminal: `mkvextract tracks <your_mkv_video> <track_numer>:<subtitle_file.srt>`

Use `mkvinfo` to get information about tracks.

Using this utility you can extract any track, even audio or video.


`flip`

参考:

* http://www.ahowto.net/linux/how-to-extract-embedded-subtitles-srt-ass-from-mkv-files/
* http://askubuntu.com/questions/452268/extract-subtitle-from-mkv-files

导出的字幕可以用于字幕合并

字幕在线合并: http://www.dualsubtitles.com/

为什么只有idx和sub文件导出成功呢? srt文件没有导出?

得到 idx和sub文件后使用 工具

```
Here it is: http://home.gna.org/subtitleeditor/
...or type "subtitle editor" into the search box in Ubuntu Software Center.
```

来合并成为srt文件? **还是不行....**

还这个方法试试 windows下的 http://jingyan.baidu.com/article/cb5d610530acea005d2fe010.html 还是不行
 因为是包含字体等信息的高级字幕,导出来都是二进制格式
