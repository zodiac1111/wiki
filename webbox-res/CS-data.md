# cgi与客户端数据交互

实用工具:

* json数据格式化 http://jsoneditoronline.org/
* chrome 开发者工具

# 串口
## 设置
### 客户端请求(post)

	file=uart&action=set&COM=1&baud_rate=0&data_bits=1&stop_bits=0&check=1&timeout=500&collect_interval=1000&each_frm_stop_time=50&COM=2&baud_rate=5&data_bits=1&stop_bits=0&check=0&timeout=500&collect_interval=1000&each_frm_stop_time=50

解释:

* file=uart 串口
* action=set 设置串口参数
* 实际数据数组,元素1
 * COM=1 串口号 base on 1 
 * baud_rate=0 模特率,list中的序号 base on 0
 * data_bits=1 数据位,list中的序号 base on 0
 * stop_bits=0 停止位,list中的序号 base on 0
 * check=1 检验位,同上
 * timeout=500 超时,字符串,单位毫秒 
 * collect_interval=1000 单位毫秒
 * each_frm_stop_time=50 单位毫秒
* 元素2.定义同上


### 服务器响应(json)

	{"ret":"success"}

解释:

* ret:success-成功,其他-失败

## 读取

### 客户端请求(post)

	file=uart&action=get

解释:

* file=uart 串口
* action=get 读取数据

### 服务器响应(json)

	{"data_bits":["7","8"],"stop_bits":["1","2"],"check":["none","odd","even"],"baud_rate":["300","600","1200","2400","4800","9600","19200","38400","57600","115200"],"data":[{"each_frm_stop_time":"50","collect_interval":"1000","timeout":"500","check":"1","stop_bits":"0","data_bits":"1","baud_rate":"0"},{"each_frm_stop_time":"50","collect_interval":"1000","timeout":"500","check":"0","stop_bits":"0","data_bits":"1","baud_rate":"5"}]}

解析:

	{
	  "data_bits": [ //数据位,填充到list中
	    "7",
	    "8"
	  ],
	  "stop_bits": [ //停止位,填充到list
	    "1",
	    "2"
	  ],
	  "check": [ //检验位,填充到list
	    "none",
	    "odd",
	    "even"
	  ],
	  "baud_rate": [ //波特率,填充到list
	    "300",
	    "600",
	    "1200",
	    "2400",
	    "4800",
	    "9600",
	    "19200",
	    "38400",
	    "57600",
	    "115200"
	  ],
	  "data": [ //实际数据
	    { //数组1
	      "each_frm_stop_time": "50", //毫秒
	      "collect_interval": "1000", //毫秒
	      "timeout": "500", //超时,单位毫秒
	      "check": "1", //校验位,list中的序号,base on 0
	      "stop_bits": "0", // 停止位,同上
	      "data_bits": "1", //数据位,同上
	      "baud_rate": "0" //波特率,同上
	    },
	    { //数组2,定义同上
	      "each_frm_stop_time": "50",
	      "collect_interval": "1000",
	      "timeout": "500",
	      "check": "0",
	      "stop_bits": "0",
	      "data_bits": "1",
	      "baud_rate": "5"
	    }
	  ]
	}

# 网络
## 设置
### 客户端请求(post)
### 服务器响应(json)
## 读取
### 客户端请求(post)
### 服务器响应(json)

# 通道
## 设置
### 客户端请求(post)
### 服务器响应(json)
## 读取
### 客户端请求(post)

	file=channel&action=get

解释

* file=channel 通道
* action=get 读取数据

### 服务器响应(json)

	{"type":["uart","network"],"protocol":["growatt inverter","chint inverter(modbus)","chint inverter(old)","chint inverter(1.5kW~10kW)","sungrow inverter(modbus)","sungorw inverter(old)","samil power(SolarRiver/SolarLake/SPM/SEM-EN)","innovpower inverter","danfoss inverter(ComLynx)","da zu inverter","altenergy-power ecu","xph intersensor","dlt645-1997","dlt645-2007"],"data":[{"enable":"1","channel_id":"1","type":"0","port":"1","protocol":"0"},{"enable":"1","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"0","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"0","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"0","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"0","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"0","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"0","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"0","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"0","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"0","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"0","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"1","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"0","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"0","channel_id":"2","type":"0","port":"2","protocol":"1"},{"enable":"0","channel_id":"2","type":"0","port":"2","protocol":"1"}]}

解释

	{
	  "type": [ // 填充到通道类型list,供选择
	    "uart",
	    "network"
	  ],
	  "protocol": [ // 对下采集协议名称.填充到协议list,供选择
	    "growatt inverter",
	    "chint inverter(modbus)",
	    "chint inverter(old)",
	    "chint inverter(1.5kW~10kW)",
	    "sungrow inverter(modbus)",
	    "sungorw inverter(old)",
	    "samil power(SolarRiver/SolarLake/SPM/SEM-EN)",
	    "innovpower inverter",
	    "danfoss inverter(ComLynx)",
	    "da zu inverter",
	    "altenergy-power ecu",
	    "xph intersensor",
	    "dlt645-1997",
	    "dlt645-2007"
	  ],
	  "data": [ // 实际数据部分
	    { // 通道数组 1
	      "enable" : "1", //通道是否使能,1使能,0不使能
	      "channel_id": "1", //通道ID,有则使能,无则不使能
	      "type": "0", // 序号.该通道类型,从上面通道类型list中选择,base on 0
	      "port": "1", // 端口,数值型,表示comX(UART)或者端口号(Network)
	      "protocol": "0" // 序号,该通道使用的采集协议.从上面协议list中选择 base on 0
	    },
	    { // 通道数组 2,定义同上
	      "enable" : "1",
	      "channel_id": "2",
	      "type": "0",
	      "port": "2",
	      "protocol": "1"
	    }
	    ... // 16个数组
	  ]
	}

# 设备
## 设置
### 客户端请求(post)
### 服务器响应(json)
## 读取
### 客户端请求(post)

	file=device&action=get

解释

* file=device 设备
* action=set 设置

### 服务器响应(json)

	{"type":["Inverter","Environment","Ammeter","Combiner box"],"data":[[{"enable":"1","addr":"1","type":"0","name":"INV1-1"},{"enable":"0","addr":"2","type":"0","name":"INV1-2"},{"enable":"1","addr":"3","type":"0","name":"INV1-3"}],[{"enable":"1","addr":"1","type":"0","name":"INV2-1"},{"enable":"1","addr":"2","type":"0","name":"INV2-2"},{"enable":"1","addr":"3","type":"0","name":"INV2-3"}],[{"enable":"0","addr":"10066","type":"4","name":"Meter"},{"enable":"1","addr":"c1123","type":"3","name":"ENV"}],[{"enable":"1","addr":"1","type":"2","name":"Other1"}]]}

解释

	{
	  "type": [ //设备类型list
	    "Inverter",
	    "Environment",
	    "Ammeter",
	    "Combiner box"
	  ],
	  "data": [ //设备数据,二维数组,第一维通道,第二维设备
	    [ //通道1
	      { //设备1
	        "enable": "1", //使能 1-使能,0-不使能
	        "addr": "1", //地址,一个通道地址应该(但是不必须)唯一,电表的可能很长,但是使用序号识别设备好.
	        "type": "0", //类型,序号.从设备类型list中获得
	        "name": "INV1-1" //设备名称
	      },
	      { //设备2
	        "enable": "0",
	        "addr": "2",
	        "type": "0",
	        "name": "INV1-2"
	      },
	      { //设备3
	        "enable": "1",
	        "addr": "3",
	        "type": "0",
	        "name": "INV1-3"
	      }
	    ],
	    [ //通道2
	      { //设备1
	        "enable": "1",
	        "addr": "1",
	        "type": "0",
	        "name": "INV2-1"
	      },
	      { //设备2
	        "enable": "1",
	        "addr": "2",
	        "type": "0",
	        "name": "INV2-2"
	      },
	      {
	        "enable": "1",
	        "addr": "3",
	        "type": "0",
	        "name": "INV2-3"
	      }
	    ],
	    [ //通道3
	      { //设备1
	        "enable": "0",
	        "addr": "10066",
	        "type": "4",
	        "name": "Meter"
	      },
	      { //设备2
	        "enable": "1",
	        "addr": "c1123",
	        "type": "3",
	        "name": "ENV"
	      }
	    ],
	    [ //通道4
	      { //设备1
	        "enable": "1",
	        "addr": "1",
	        "type": "2",
	        "name": "Other1"
	      }
	    ]
	  ]
	}

# 数据中心
## 设置
### 客户端请求(post)

	file=centers&action=set

解释

* file=centers 数据中心
* action=set 设置

### 服务器响应(json)
## 读取
### 客户端请求(post)

	file=centers&action=get

解释

* file=centers 数据中心
* action=get 读取数据

### 服务器响应(json)

	{"mode":["gprs","ethernet"],"center_type":["Chint Monitor Center","Golden-sun Data Center","Building Dep"],"data":[{"mode":"0","center_type":"0","center_ip":"60.12.137.82","port":"80","interval":"5","sn_pwd":"123456","aes":"123456","start_hour":"5","end_hour":"19"}]}

解释

	{
	  "mode": [ // 模式list,供填充,通过无线gprs或者有线以太网
	    "gprs",
	    "ethernet"
	  ],
	  "center_type": [ //数据中心list,供填充,可选中自,金太阳,住建部
	    "Chint Monitor Center",
	    "Golden-sun Data Center",
	    "Building Dep"
	  ],
	  "data": [ //实际数据部分(该数组目前就一个元素,即仅关联一个数据中心)
	    {
	      "mode": "0", // 序号,取模式list中的模式. base on 0
	      "center_type": "0", // 序号,取数据中心list中的哪个数据中心. base on 0
	      "center_ip": "60.12.137.82", // 数据中心ip
	      "port": "80", // 数组中心端口
	      "interval": "5", //数值,数据上传周期(分钟).5(1)~60.整数
	      "sn_pwd": "123456",//字符,单元密码
	      "aes": "123456", //字符,aes密码(chitic数据中心不需要?)
	      "start_hour": "5", //开始上传时刻(小时),24小时制,当地时间.取 0-23
	      "end_hour": "19" //停止上传时刻(小时),24小时制,当地时间.取0-23 必须大于开始时刻
	    }
	  ]
	}
