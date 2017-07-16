* [视频演示,超酷炫](https://www.youtube.com/watch?v=VOC3huqHrss)

# 资料

* [官方](https://pjreddie.com/darknet/yolo/)
* 1 [Yolo darknet训练自己的数据集教程(Newest 2016.12.23)](https://jinfagang.github.io/2017/02/02/2017-12-22-yolo%E6%95%99%E7%A8%8B/)
* [Blazing Fast! 使用YOLO+Darknet进行目标检测](http://xxuan.me/2016-12-26-YOLO-darknet.html)


# 介绍

基于`darknet`,需要cuda GPU运算, 需要opencv

按照资料1 能简单检测到几个物体. 算是 helloworld 成功.

使能cudnn, 依赖cuda

6的版本存在 __mempcpy 问题 看来是nvcc inline 的问题, 参考 [这个](https://groups.google.com/forum/#!topic/darknet/vqN7uHu8h1U)


# 笔记

## 显存不足
测试(或者训练)时出现以下错误

```
    7 max          2 x 2 / 2    52 x  52 x 128   ->    26 x  26 x 128
    8 CUDA Error: out of memory
darknet: ./src/cuda.c:36: check_error: Assertion `0' failed.
```

解决方法:
1. 买块高级的显卡(笑
2. 修改配置`cfg`文件,减少`batch`和`subdivisions`的值 [参考](https://stackoverflow.com/questions/43614686/cudaout-of-memory-error-on-darknet-yolov1)
