# datatable 使用笔记

## 服务端

## 默认排序

> http://www.datatables.net/forums/discussion/9816/noobie-help-server-side-default-sorting/p1

`aaSorting`

```js
$(document).ready(function() {
    $('#dietTable').dataTable( {
    "bProcessing": true,
    "bServerSide": true,
    "sAjaxSource": "serverProcessing.php"
    "aaSorting": [[ 0, "desc" ]]
    } );
} );
```