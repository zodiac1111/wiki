# 级联分类器训练 cascade_classifier_training

* 官方文档 英文 http://docs.opencv.org/doc/user_guide/ug_traincascade.html
* 中文官方
* 翻译和理解 http://1.guotie.sinaapp.com/?p=329
* 中文实战,有作者的自己的过程,windows http://blog.csdn.net/linj_m/article/details/16331347

# 自己理解

# 生成ves

## 生成??

## 方式1

一个一个图片生成,太麻烦,放弃

## 方式2

使用 info文件

### 创建info文件

### 创建ves

    opencv_createsamples -info info.dat -w 48 -h 48 -vec 1.vec
    opencv_createsamples -info <信息文件> -w <宽> -h <高> -vec 输出的vec文件


# 训练(花费时间)
正样本数 31 测试中

图片不能过大 48*48 测试通过