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