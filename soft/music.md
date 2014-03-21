# 音频转换工具 

> 参考 http://www.linuxidc.com/Linux/2008-10/16212.htm

# FLAC 和 WAV 之间相互转换
flac -> wav
``` 
flac -d <输入_flac文件> -o <输出_wav文件>
```
wav -> flac 
```
flac <输入_wav文件> <输出_flac文件> -<压缩比率 1-8 , 默认为 5>
```