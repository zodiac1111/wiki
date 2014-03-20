# 级联分类器训练 cascade_classifier_training

* 官方文档 英文 http://docs.opencv.org/doc/user_guide/ug_traincascade.html
* 官方文档 中文 http://www.opencv.org.cn/opencvdoc/2.3.2/html/doc/user_guide/ug_traincascade.html
* 翻译和理解 http://1.guotie.sinaapp.com/?p=329
* 中文实战,有作者的自己的过程,windows http://blog.csdn.net/linj_m/article/details/16331347

# 自己理解

# 生成ves

## 生成??

## 方式1

一个一个图片生成,太麻烦,放弃

## 方式2

使用 info文件,

自己写了个程序批量手动生成.

### 创建info文件

    find info -name "*.jpg"  | xargs -i ./mkinfo {}

使用自己编译的mkinfo工具,参见这里(todo)

### 创建ves

    opencv_createsamples -info info.dat -w 48 -h 48 -vec 1.vec
    opencv_createsamples -info <信息文件> -w <宽> -h <高> -vec 输出的vec文件

输出类似:
```
Info file name: info.dat
Img file name: (NULL)
Vec file name: 1.vec
BG  file name: (NULL)
Num: 1000
BG color: 0
BG threshold: 80
Invert: FALSE
Max intensity deviation: 40
Max x angle: 1.1
Max y angle: 1.1
Max z angle: 0.5
Show samples: FALSE
Width: 48
Height: 48
Create training samples from images collection...
info.dat(32) : parse errorDone. Created 31 samples
```
个人测试 图片不能过大 48*48 测试通过,96*96 通过

# 训练(花费时间)
正样本数 31 测试中


```
opencv_traincascade -vec 1.vec -data train -bg bg.txt -numPos 31 -w 24 -h 24 -numNeg 100    -featureType LBP -minHitRate 0.99 -maxFalseAlarmRate 0.5 -maxDepth 2 -maxWeakCount 3 -precalcValBufSize 1024
```
解释
```
opencv_traincascade -vec 输入的ves文件 -data 输出的路径 -bg 负样本信息文件 \
-numPos 样本个数 -w 宽 -h 高 -numNeg 负样本个数 -featureType 类型,我选LBP \
-minHitRate 最小比例 -maxFalseAlarmRate 警告比例 -maxDepth 最大路径深度,1=二叉树 -maxWeakCount 不知道 \
-precalcValBufSize 内存(兆)貌似不指定也可以,指定大点似乎会快点.
```

* `-w 宽 -h 高` 必须更上面的 "创建ves" `Width: 48 Height: 48`一样大
* `-precalcValBufSize` 内存(兆)貌似不指定也可以,指定大点似乎会快点.

# 使用例子 

* 中文(官方)  http://www.opencv.org.cn/opencvdoc/2.3.2/html/doc/tutorials/objdetect/cascade_classifier/cascade_classifier.html