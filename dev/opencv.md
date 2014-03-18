#opencv

opencv编译过程记录 2013-12-31 version 2.4.8

##下载 

官网 http://opencv.org/

Linux版本下载(约80+M) http://sourceforge.net/projects/opencvlibrary/files/opencv-unix/2.4.8/opencv-2.4.8.zip/download

## 编译

解压.

cmake

    cmake .

make

    make

漫长的等待(如果上天眷顾的话)编译成功

安装

    sudo make install

更行so配置 

    ldconfig

否则可能出现编译实例通过,但运行是找不到so文件,例如

```
error while loading shared libraries: libopencv_nonfree.so.2.4: cannot open shared object file: No such file or directory
```

## 期间问题

## 例子(物体检测)

编译,指定lib库搜索路径,运行

### 特征检测 Feature Detection

官方例子: http://docs.opencv.org/doc/tutorials/features2d/feature_detection/feature_detection.html

编译:

运行

### 查找一个已知物体


    g++ cornerSubPix_Demo.cpp  -lopencv_core -lopencv_highgui -lopencv_imgproc

Features2D + Homography to find a known object


http://docs.opencv.org/doc/tutorials/features2d/feature_homography/feature_homography.html


```
g++ XXX.cpp -lopencv_core -lopencv_features2d -lopencv_highgui -lopencv_calib3d -lopencv_nonfree  -lopencv_flann
```

测试图片路径 /samples/c