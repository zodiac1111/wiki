# 测试

测试gollum各种语法(markdown)以及各种特性的页面.

## 图片测试 
### 1
```bash
![外部链接](https://cdn1.iconfinder.com/data/icons/onebit/PNG/onebit_34.png)
```
![外部链接](https://cdn1.iconfinder.com/data/icons/onebit/PNG/onebit_34.png)

### 2
```bash
[[https://cdn1.iconfinder.com/data/icons/onebit/PNG/onebit_34.png]]
```
[[https://cdn1.iconfinder.com/data/icons/onebit/PNG/onebit_34.png]]

### 3
[[gollum.png]]

![asdasd](/gollum.png)

[[/gollum.png|align=right]]

## 文字样式
* <font color=red>红色文字</font> `<font color=red>红色文字</font>`
* <b>粗体文字</b> `<b>粗体文字</b>`
* <i>斜体文字</i> `<i>斜体文字</i>`
* **粗体文字** `**粗体文字** `
* *斜体文字* `*斜体文字*`

## 标题
4. <h1>一级标题</h1> 
5. <h2>二级标题</h2>
6. <h3>三级标题</h3>
7. <h4>四级标题</h4>
8. <h5>五级标题</h5>
9. <h6>六级标题</h6>
10. <h7>七级标题</h7>
11. # 标题

## 分割线

=====

-----


## 类表

有序列表

1. 11111111
2. 22222222
3. 33333333
4. 44444444

无序列表

* 苹果
* 香蕉
* 葡萄
* 草莓


## 链接

* https://google.com
* [Google](https://google.com)

## 视频
1

<div class="video-container">
http://player.youku.com/player.php/sid/XNDQ4ODQxNTI0/v.swf
</div>

2

<div class="video-container">
  <embed src="http://player.youku.com/player.php/sid/XNDQ4ODQxNTI0/v.swf" allowFullScreen="true" quality="high" width="480" height="400" align="middle" allowScriptAccess="always" type="application/x-shockwave-flash"></embed>
</div>

3

<div class="github-widget" data-repo="zodiac1111/dotvim"></div>

4

<iframe height=498 width=510 src="http://player.youku.com/embed/XNTUzNDA5MTQ4" frameborder=0 allowfullscreen></iframe>

5

<embed src="http://player.youku.com/player.php/sid/XNTUzNDA5MTQ4/v.swf" allowFullScreen="true" quality="high" width="480" height="400" align="middle" allowScriptAccess="always" type="application/x-shockwave-flash"></embed>

eof
