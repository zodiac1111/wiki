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

# 分割APE/CUE镜像 
##  先解码成WAV格式
```
mac CDImage.ape CDImage.wav -d 
```
再进行切割
```
bchunk -w CDImage.wav CDImage.cue output 
```
或 
```
shnsplit -f CDImage.cue CDImage.wav
```
## 也可以直接切割
```
shnsplit -f CDImage.cue -i ape 
```
## 切割并转换到其他格式(FLAC/mp3)
```
shnsplit -f CDImage.cue -i ape -o flac CDImage.ape 
shnsplit -f CDImage.cue -i ape \
-o "cust ext=mp3 lame -b 320 - %f" CDImage.ape 
```