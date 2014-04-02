# datatable 使用笔记

## 服务端

`sAjaxSource`

## 默认排序

* http://www.datatables.net/forums/discussion/9816/noobie-help-server-side-default-sorting/p1
* https://datatables.net/usage/columns

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

##列定义

`aoColumnDefs`(推荐,更先进) 或者`aoColumns`

```js
// Using aoColumnDefs
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumnDefs": [
      { "sSortDataType": "dom-text", "aTargets": [ 2, 3 ] },
      { "sType": "numeric", "aTargets": [ 3 ] },
      { "sSortDataType": "dom-select", "aTargets": [ 4 ] },
      { "sSortDataType": "dom-checkbox", "aTargets": [ 5 ] }
    ]
  } );
} );
 
 
// Using aoColumns
$(document).ready( function() {
  $('#example').dataTable( {
    "aoColumns": [
      null,
      null,
      { "sSortDataType": "dom-text" },
      { "sSortDataType": "dom-text", "sType": "numeric" },
      { "sSortDataType": "dom-select" },
      { "sSortDataType": "dom-checkbox" }
    ]
  } );
} );
```

定义数据源项

```js
"mData" : "item_name",
"aTargets" : [2],
```

## 单元格整理格式

`mRender`

```js
"mRender" : function(data, type, full) {
    // 'full' is the row's data object, and 'data' is this column's data
    // e.g. 'full[0]' is the comic id, and 'data' is the comic title
	if(data=="0"){
		return '<span class="tag tgwtb">wtb</span>';
	}else if(data=="1"){
		return '<span class="tag tgwts">wts</span>';
	}else if(data=="2"){
		return '<span class="tag tgwtt">wtt</span>';
	}else{
		return "E:"+data;
	}
}
```

## 显示页码样式

显示页码: `sPaginationType`

页码样式不正确:

> http://datatables.net/forums/discussion/126/spaginationtype-full_numbers-buttons-not-being-shown/p1
