# ie ajax 错误

>  http://www.im87.cn/blog/256

淘一族(www.taoyizu.net)改版中遇到个奇怪的问题，有个页面需要用jquery的ajax获取数据，在chrome和ff下都能正确获取并显示数据，代码段如下：

```js
[

$.ajax({
    url: "/item/getComments",
    dataType: "json",
    data: {"iid": "123456", "nick": "xlight"},
    success: function (data) {
        $(".comments-total").html(data.total);
        $(".comments-body").html(data.list);
    }
});
]  
```
用IE自带的debug工具查看，显示数据已经成功获取(status:200并有数据返回)，那就奇怪了……

给ajax加上error回调：
```js
[

$.ajax({
    url: "/item/getComments",
    dataType: "json",
    data: {"iid": "123456", "nick": "xlight"},
    success: function (data) {
        $(".comments-total").html(data.total);
        $(".comments-body").html(data.list);
    },
    // 这里三个参数详细请看jquery手册
    error: function (a, b, c) {
        alert(c);
    }
});
]  
```
IE显示错误“Error:c00ce56e”之类的信息，google了下，大致了解到这是IE无法解析数据的原因，绝大多数是因为header中有畸形编码导致的。

知道了原因排查起来也就容易了，用IE的debug工具看了下返回数据的header，有“Content-Type text/html; charset=utf8”，可以看出这里应该是utf-8，找到后端代码改之（header发送函数，如php的header()），刷新就ok了！

另附上个其它网友的案例和解释：

http://www.iefans.net/ajax-ie-baocuo-c00ce56e/