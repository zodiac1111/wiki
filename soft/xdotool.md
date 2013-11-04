# xdotool

[xdotool](http://www.semicomplete.com/projects/xdotool/) X桌面下模拟鼠标按键等(按键精灵)

一些技巧: http://pratyeka.org/fake-x-input/

#### 模拟鼠标移动&点击
```
xdotool mousemove 807 380 
xdotool click 1 
```
#### 配合获得窗口信息
x window information 
```
xwininfo
```
#### 模拟键盘按键
```
xdotool key alt+t space;
```
#### 模拟键盘输入
```
xdotool type 'All your closed source apps are belong to us.'
```
#### 查找
```
xdotool search ???
```