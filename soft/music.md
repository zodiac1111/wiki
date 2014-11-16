# 音频转换工具 

> 参考 http://www.linuxidc.com/Linux/2008-10/16212.htm

# FLAC 和 WAV 之间相互转换
需要 flac 程序

    apt-get install flac

## flac -> wav

	flac -d <输入_flac文件> -o <输出_wav文件>

## wav -> flac 

	flac <输入_wav文件> <输出_flac文件> -<压缩比率 1-8 , 默认为 5>

最简单的

	flac demo.wav

即可在当前目录输出 demo.flac 文件

# 分割APE/CUE镜像 

需要程序 bchunk 

	ape-get install bchunk

##  先解码成WAV格式

	mac CDImage.ape CDImage.wav -d 

再进行切割

	bchunk -w CDImage.wav CDImage.cue output 

或 

	shnsplit -f CDImage.cue CDImage.wav

## 也可以直接切割

	shnsplit -f CDImage.cue -i ape 

## 切割并转换到其他格式(FLAC/mp3)

	shnsplit -f CDImage.cue -i ape -o flac CDImage.ape 
	shnsplit -f CDImage.cue -i ape \
	-o "cust ext=mp3 lame -b 320 - %f" CDImage.ape 

#APE <-> FLAC 互相转换

	shnconv -i ape -o flac CDImage.ape 
	shnconv -i flac -o ape CDImage.flac


#apple

foobar2000支持cue,utf8的编码必须包含dom

linux deadbeef 支持转换,需要相应编码器 faac lame flac 等

例如转换为apple aac.

debian 下载faac包

deadbeef右键歌曲->转换->编码器选择AAC(Nero FAAC)->右侧编辑->编辑->命令行`faac -o %o -`

deadbeef0.6.1 FAAC 1.28 下参数不对,使用命令行打开并转化出错,提示不支持w选项

```
FAAC 1.28

faac: invalid option -- 'w'
Usage: faac [options] [-o outfile] infiles ...

	<infiles> and/or <outfile> can be "-", which means stdin/stdout.

See also:
	"faac --help" for short help on using FAAC
	"faac --long-help" for a description of all options for FAAC.
	"faac --license" for the license terms for FAAC.

converter: write error (-1 bytes written out of 8000)
```
其他默认即可

%a - %t 等占位符见[这里](https://github.com/Alexey-Yakovenko/deadbeef/blob/e4960feb27ed97a34ca6442ee1a917b568c1859d/deadbeef.h#L741)