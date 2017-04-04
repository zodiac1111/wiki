# 机器学习

Machine Learning, ML

人工智能

深度学习

[训练莎士比亚例子](http://blog.csdn.net/liuxiabing150/article/details/46756147)

[知乎上的资料](https://www.zhihu.com/question/29411132)

[基于字符的神经网络](https://github.com/yoonkim/lstm-char-cnn)

目前用这个 [[https://github.com/karpathy/char-rnn]]

中间需要安装cuda [[https://developer.nvidia.com/cuda-toolkit]]

中间需要 Install CUDA

```
$ sudo apt install nvidia-cuda-toolkit
```

```shell
sudo apt install luarocks -y
sudo apt install cmake -y # 因为要编译
```

安装不上? [参考](https://github.com/torch/nngraph/issues/52)
```shell
# luarocks install nngraph 
sudo luarocks --from=https://raw.githubusercontent.com/torch/rocks/master/ install nngraph
```
```
# luarocks install luautf8
sudo luarocks --from=https://raw.githubusercontent.com/torch/rocks/master/ install luautf8
```

安装方式2

```
curl -sk https://raw.githubusercontent.com/torch/ezinstall/master/install-deps | bash  
git clone https://github.com/torch/distro.git ~/torch --recursive  
cd ~/torch; ./install.sh 
```


[安装Torch](https://torch.ch/docs/getting-started.html#_)


# 参考资料

[Torch](https://torch.ch/)

char-cnn：
https://github.com/karpathy/char-rnn
Torch:
http://torch.ch/docs/getting-started.html
The Unreasonable Effectiveness of Recurrent Neural Networks：
http://karpathy.github.io/2015/05/21/rnn-effectiveness/
汪峰老师作词机终于填坑了
http://phunters.lofter.com/post/86d56_732209