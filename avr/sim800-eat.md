# sim800 eat

sim800 芯讯通无线科技(上海)有限公司 技术文档下载 http://simcomm2m.com/service/down.aspx?proType=-1&type=-1&proName=-1&time=-1&page=18

需要注册

环境.

usb下载需要重启.

1. 软件上按下载
2. 重启板子
3. 开始下载

选usb.

调试可以使用uart2, 代码需要修改.


* mt6227 驱动需要xp系统
* 编译器 `RVCT3 1 569 armcc3.1 build 569`. 实验得4.0 也可以编译,暂时没发现有问题

```
#!/usr/bin/env sh
#!/usr/bin/env bash
# set -x

# 默认串口设备文件,可以通过传递脚本选项修改
if [ "${SHELL}" == "/bin/bash" ] ; then # host调试和板子区分
	export BLUE_TOOTH_TTY_DEV="/dev/ttyUSB0"
else
	export BLUE_TOOTH_TTY_DEV="/dev/ttyS1"
fi
SIM800_FILESYSTEM_BT_DEFAULT_DIR='C:\\User\\BtReceived\\'
dstfile="file.zip" # 文件上传的蓝牙模块的文件名,gzip 压缩后缀名应该是.gz.这里是蓝牙限制了后缀名格式
BLUE_TOOTH_BUFF="/tmp/btbuff"
# 从模块保存到linux的缓冲区
SYS_BUFF_DIR="/tmp"
IS_LOGIN="1" # 默认的登陆状态, 调试可以置1
wait_for_response_default_timeout_second=100 # 默认at指令等待响应超时百毫秒数
ret_string=""
g_curStatus=""
g_preStatus=""
lists="" # 当前配对和连接的列表
# 定义蓝牙流程状态
STATUS_IDLE=1;	# 空闲
STATUS_COMMAND=2; # 获得指令
STATUS_GETFILE=3; # 获得文件
STATUS_LOGIN=4; # 登陆
STATUS_LOGOUT=5; # 登出
STATUS_DISCONNECTION=6; # 失去连接
STATUS_PAIR=7; # 收到配对请求
STATUS_WAIT_FOR_DISCONN=8; # 等待客户端连接
STATUS_BOOT=9; # 程序最开始启动
STATUS_CONNECTIONED=10; # 已经有客户端配对了
STATUS_CONNECT=11; # 收到连接请求
STATUS_SENDTO_FILE_RESQ=12; # 发送文件给客户端后客户端返回,拒绝或接收之类
STATUS_UNKONW=100; # 未知状态

idle_time=0; # 空闲的时间,长时间空闲则登出并踢掉客户端.
MAX_IDLE_TIME=9999 ; # 最大空闲时间,超时则断开客户端, 推荐5分钟(300)或者10分钟(600)

CLIENT_ID="0"
CLIENT_NAME=""
PAIRID="6"

# 主函数
main() {
	check_option_and_do "$@" # 脚本选项
	CLOG_INFO "蓝牙端口: ${BLUE_TOOTH_TTY_DEV}"
	# 基本设置
	local s=${STATUS_BOOT}; # 设置状态机初始状态
	# 启动dd参与脚本和串口设备文件的交互,仅启动一次,调试时手动启动并注销以下一行
	# start_data_rt_subshell;
	set_dev_attr; # 模块通讯串口层设置 (似乎不需要特别设置?)
	turnoff_echo; # 模块相关,关闭回显(显示发送的at命令)
	show_fs_info; # 显示一些文件系统相关信息
	clean_bt_recv_folder; # 清理一下接收文件夹.保证空间.长文件可能删除不了,也忽略

	# unpair # debug时用,删除所有配对的信息,恢复原始状态,开发时很有用
	# 蓝牙相关开始,启动蓝牙.启动失败脚本退出
	start_bluetooth;ret=$?
	if [ $ret -ne ${COMMAND_RET_OK} ] ; then
		CLOG_ERR "启动蓝牙失败"
		exit 100;
	fi

	# 设备配对,数据连接层循环.因为状态取得方式需要多行判断所以不能并入状态机循环
	while true ; do
		#scan # 调试用:扫描
		#if ( isNowHasClientConnected ) ; then
		#	s=${STATUS_CONNECTIONED};
		#fi
		CLOG_INFO "进入状态机循环"
		g_preStatus="${g_curStatus}"; # 更新状态
		while true ; do # 数据接收层循环,可以使用通用状态机
			idle_back=${idle_time};idle_time=0 # 任何非闲置动作都重置闲置定时器
			get_status;s=$? # 读取蓝牙输入,判断获取状态.限一行状态
			if [ $s -eq ${STATUS_BOOT} ] ; then
				echo "初始化模块"
			elif [ $s -eq ${STATUS_IDLE} ] ; then # 闲置,空闲
				DO_idle;idle_time=${idle_back} # 备份的闲置定时(计数)器用户闲置操作
				if [ ${idle_time} -gt ${MAX_IDLE_TIME} ] ; then # 空闲重置客户端
					DO_tick_client; idle_time=0;
				fi
			elif [ $s -eq ${STATUS_PAIR} ] ; then # 客户端发起配对请求
				DO_accept_pair;
			elif [ $s -eq ${STATUS_CONNECT} ] ; then # 客户端发起连接请求
				DO_accept_connect ; DO_get_client_info;
			elif [ $s -eq ${STATUS_CONNECTIONED} ] ; then # 有客户端连接
				DO_get_client_info;
			elif [ $s -eq ${STATUS_DISCONNECTION} ] ; then # 客户端断开连接
				DO_reset_client_info
			elif [ $s -eq ${STATUS_COMMAND} ] ; then # 获得数据(执行命令模式,客户端发送指令)
				DO_eval_command;
			elif [ $s -eq ${STATUS_GETFILE} ] ; then # 文件传输,客户端->板子
				DO_receive_file
			elif [ $s -eq ${STATUS_SENDTO_FILE_RESQ} ] ; then # 文件传输响应(发送文件)板子->客户端
				DO_send_file_to_client
			else # 没有预期到的信息
				vhex=$(phex ${ret_string});
				echo -e "${CL_YELLOW}未知信息${CL_END}:${CL_RED}${ret_string}${CL_END}"
				echo "${vhex}";nop;
			fi
			sleep 1 ; idle_time=$(expr ${idle_time} + 1)
		done
	done
}

# 检查选项并且进行相关设定
check_option_and_do () {
	if [ $# -eq 1 ] ;then # 脚本名 <指定设备文件>
		export BLUE_TOOTH_TTY_DEV="$1"
	fi
	return 0;
}
# 关闭回显,不再显示发送的命令,调试是否可以开着回显确认信息
turnoff_echo () {
	bt_at_send "ATE0"
	bt_at_get_rsp "OK\|ERROR" 10;ret=$?
}

# 显示蓝牙文件系统一些信息
show_fs_info () {
	# 查看文件系统空间
	bt_at_send 'AT+FSMEM'
	bt_at_get_rsp "OK\|ERROR" 10;ret=$?
	# ls一下接收目录
	bt_at_send "AT+FSLS=${SIM800_FILESYSTEM_BT_DEFAULT_DIR}"
	bt_at_get_rsp "OK\|ERROR" 10;ret=$?
}
# 打开子shell 获取数据. & 异步处理
start_data_rt_subshell () {
	(
		CLOG_INFO "启动数据管道 ${BLUE_TOOTH_TTY_DEV}"
		while true; do dd if=${BLUE_TOOTH_TTY_DEV} bs=1 >>/tmp/btbuff ; done
		# 调试时可以显示内容
		# while true; do dd if=${BLUE_TOOTH_TTY_DEV} bs=1 | tee -a /tmp/btbuff ; done
		CLOG_INFO "结束数据管道 ${BLUE_TOOTH_TTY_DEV}"
	) &
}

# 提示消息:需要登陆
show_need_login () {
	CLOG_WARN "需要登陆"
}
# 返回字符串判断
# @param[in] 从这个字符串中判断
# @param[in] $@ 判断是否符合这个正则表达式
# @retval 0 符合条件
# @retval 其他 不符合条件
isRet () {
	echo ${ret_string} | grep -q -e "$@" ;ret=$?
	#CLOG_INFO "字符串结果 ${ret_string} 里 $@ , $ret"
	return $ret
}
# 定义状态极获取状态动作
# @param[in] 全局变量 g_curStatus
# @param[out]全局变量 g_preStatus
# @retval 定义的状态
get_status () {
	local s=${STATUS_UNKONW}
	# 只接受一行就返回,所以一些其他状态比如客户端是否连接等不能用这个取得,
	# 需要使用专门的状态取得函数.
	bt_at_get_rsp ".*" 10;ret=$?
	if [ "x${g_curStatus}" == "x${g_preStatus}" ] ; then
		s=${STATUS_IDLE}
	elif ( echo "${g_curStatus}" | grep -q -e "+BTPAIRING:" ) ; then
		s=${STATUS_PAIR}
	elif ( echo "${g_curStatus}" | grep -q -e "+BTCONNECTING:" ) ; then
		s=${STATUS_CONNECT}
	elif ( echo "${g_curStatus}" | grep -q -e "+BTCONNECT:" ) ; then
		s=${STATUS_CONNECTIONED}
	elif ( echo "${g_curStatus}" | grep -q -e "+BTDISCONN:" ) ; then
		s=${STATUS_DISCONNECTION}
	elif ( echo "${g_curStatus}" | grep -q -e "+BTOPPPUSHING:" ) ; then
		s=${STATUS_GETFILE}
	elif ( echo "${g_curStatus}" | grep -q -e "+BTOPPPUSH:" ) ; then
		s=${STATUS_SENDTO_FILE_RESQ}
	elif ( echo ${g_curStatus} | grep -q -e "+BTSPPDATA:" ) ; then
		s=${STATUS_COMMAND}
	else
		CLOG_ERR "获得未知状态:${g_curStatus}"
		s=${STATUS_UNKONW}
	fi
	g_preStatus="${g_curStatus}"; # 更新状态
	return $s
}

# 文件系统:从系统到模块的文件系统.
# 复制系统的文件到蓝牙模块的文件系统中.小于10*1024字节
# 输入 $1 系统目录文件全路径
# 输出 蓝牙目录下 ${dstfile} 文件
fs_sys2bt () {
	sysfile="$1"

	zipdfile="/tmp/${dstfile}"
	bt_dstfile="${SIM800_FILESYSTEM_BT_DEFAULT_DIR}${dstfile}"
	# 1. 压缩
	gzip "${sysfile}" -c > "${zipdfile}" ;ret=$?
	#cat "${sysfile}"  > "${zipdfile}";ret=$? # 调试用
	if [ $ret -ne ${COMMAND_RET_OK} ] ; then
		CLOG_ERR "gzip 失败 $ret"
		return 100;
	fi
	zfsize=$(ls -l "${zipdfile}" |awk  '{print $5}')
	if [ $zfsize -gt 10240 ] ; then # 蓝牙文件系统指令限制
		CLOG_ERR "文件过大无法传输 $zfsize"
		return 110
	fi
	# 2. 传输(创建)到蓝牙模块文件系统
	# 2.1 创建蓝牙文件系统文件,几乎不能一次创建成功,不知什么原因,手动操作则一次即可
	# 	似乎2次就能成功.就暂时这样做着吧.创建文件,
	bt_at_send "AT+FSCREATE=${bt_dstfile}"
	bt_at_get_rsp "OK" 1000;ret=$?
	if [ $ret -ne ${COMMAND_RET_OK} ] ; then
		CLOG_WARN "创建文件超时 $ret 尝试第2次";mdelay 500;
		bt_at_send "AT+FSCREATE=${bt_dstfile}"
		bt_at_get_rsp "OK" 1000;ret=$?
		if [ $ret -ne ${COMMAND_RET_OK} ] ; then
			CLOG_WARN "创建文件超时 $ret 尝试第3次";mdelay 500;
			bt_at_send "AT+FSCREATE=${bt_dstfile}"
			bt_at_get_rsp "OK" 1000;ret=$?
			if [ $ret -ne ${COMMAND_RET_OK} ] ; then
				CLOG_ERR "创建文件超时 $ret"
				return 120;
			fi
		fi
		#
	fi

	# 2.2 写文件
	local skip=5 # 跳过开头的"^M^J>" 共5个字节 (仅debianx86host机子上有这种情况,板子上没有这种情况?)
	# zfsize=$(expr ${zfsize} + ${skip}) # 仅在主机调试时需要
	bt_at_send "AT+FSWRITE=${bt_dstfile},0,${zfsize},200"
	bt_at_get_rsp ">" 2000;ret=$?
	if [ $ret -ne ${COMMAND_RET_OK} ] ; then
		CLOG_ERR "fswrite 失败 $ret"
		return 120;
	fi
	# 使用二进制方式写到串口: dd可以保证二进制.0x00等不被转义
	# 写入的文件开头会有5个字节的无用数据
	dd if="${zipdfile}" of="${BLUE_TOOTH_TTY_DEV}" bs=${zfsize}
	# bt_at_send "12344444444444444444444444444" # 调试用
	bt_at_get_rsp "OK" 2000;ret=$?
	return 0;
}

# 文件系统:从蓝牙模块到系统的文件系统
fs_bt2sys () {
	local sfile=$1 ; # 源文件(文件名)
	local ssize=$2 ; # 源文件大小
	local dfile="recive-${sfile}" ; # 目标文件
	# 读取文件到shell变量
	bt_at_send "AT+FSREAD=${SIM800_FILESYSTEM_BT_DEFAULT_DIR}${sfile},0,${ssize},0"
	bt_at_get_rsp "OK" 300 ${SAVE_MSG_TO_FILE};ret=$?
	if [ ${ret} -ne ${COMMAND_RET_OK} ] ; then
		CLOG_ERR "读取文件失败 ret=${ret}"
		return 101
	fi
	echo ${ret_string} | grep -q -e "OK" | ret=$?
	if [ ${ret} -ne ${COMMAND_RET_OK} ] ; then
		CLOG_ERR "读取文件错误"
		return 102
	fi
	# 转移到Linux文件系统
	# 跳每块一个字节,过前面 \r\n (0x0d 0x0a)2个字节.
	local skipbyte=2
	dd if="${BLUE_TOOTH_BUFF}.tmpsavefile" \
		bs=1 count=${size} skip=${skipbyte} \
		of="${SYS_BUFF_DIR}/${dfile}";ret=$?
	if [ ${ret} -ne ${AT_RET_OK} ] ; then
		CLOG_ERR "写入到系统filesystem失败"
		return 103
	fi
	g_preStatus="${g_curStatus}";
	CLOG_INFO "写入到系统fs成功: ${SYS_BUFF_DIR}/${dfile}"
	return 0;
}

# 打印状态信息
pStatusMsg () {
	if [ $s -eq ${STATUS_BOOT} ] ; then
		echo "正在启动"
	elif [ $s -eq ${STATUS_IDLE} ] ; then
		echo "空闲"
	fi
	return 0;
}

# 状态机动作:踢掉客户端
DO_tick_client () {
	if [ "${CLIENT_ID}" != "0" ] ; then
		CLOG_INFO "踢掉客户端:${CLIENT_ID}"
		bt_at_send "AT+BTDISCONN=1"
		bt_at_get_rsp "OK" ;ret=$?
		if [ $ret -ne ${COMMAND_RET_OK} ] ; then
			CLOG_ERR "失败"
			return 100;
		fi
		CLOG_INFO "成功"
		g_preStatus="${g_curStatus}"; # 更新状态
		return 0
	fi
}

# 状态机动作:客户端断开,重置客户端信息
DO_reset_client_info () {
	if [ "${CLIENT_ID}" != "0" ] ; then
		echo "CLIENT_ID=${CLIENT_ID}"
		CLOG_WARN "断开客户端:[${CLIENT_ID}]:${CLIENT_NAME}"
		CLIENT_ID="0"
		CLIENT_NAME=""
	fi
	IS_LOGIN="0"
}
DO_send_file_to_client () {
	if [ "${FILE_DIA}" == "SEND_WAIT_FOR_CLIENT" ] ; then
		FILE_DIA="IDLE";
		if ( isRet "+BTOPPPUSH: 0" ) ; then
			CLOG_ERR "发送失败"
			g_preStatus="${g_curStatus}";
			return 110
		elif ( isRet "+BTOPPPUSH: 1" ) ; then
			CLOG_INFO "发送成功"
			g_preStatus="${g_curStatus}";
			return 0
		elif ( isRet "+BTOPPPUSH: 2" ) ; then
			CLOG_ERR "服务器出错"
			return 101
		else
			CLOG_ERR "客户端接收文件错误"
			return 102
		fi
	fi
}

# @get <配对序号> <文件全路径>
# @get <文件全路径> 已经配对过并连上的客户端
command_send_file () {
	# 根据参数数量判断参数类型
	local who="$1"
	local file="$2"
	filename="${dstfile}" # 蓝牙文件系统的文件名
	# 1. 从linux文件系统复制到蓝牙文件系统
	fs_sys2bt ${file};ret=$?
	if [ $ret -ne ${COMMAND_RET_OK} ] ; then
		CLOG_ERR "fs_sys2bt 失败"
		FILE_DIA="IDLE"
		return 100;
	fi
	PAIRID="${who}"
	bt_at_send "AT+BTOPPPUSH=${PAIRID},${SIM800_FILESYSTEM_BT_DEFAULT_DIR}${filename}"
	bt_at_get_rsp "OK" 1000 ;ret=$?
	if [ $ret -ne ${COMMAND_RET_OK} ] ; then
		CLOG_ERR "发送文件失败 command_send_file,ret=$ret"
		FILE_DIA="IDLE"
		return 110
	fi
	if ( isRet "+BTOPPPUSH: 0" ) ; then
		CLOG_ERR "发送失败"
		FILE_DIA="IDLE"
		g_preStatus="${g_curStatus}";
		return 120
	fi
	CLOG_INFO "等待客户端接收"
	#if ( isNowHasClientConnected ) ; then send_to_client_tty "文件已发送" ; fi
	g_preStatus="${g_curStatus}";
	FILE_DIA="SEND_WAIT_FOR_CLIENT"
	return 0;
}
# 状态机动作:保存文件
DO_receive_file () {
	local isDeleteReciveFileInBlueToothMod=0 ; #是否删除蓝牙模块fs中的文件
	client=$(get_opppushing_clientname)
	filename=$(get_opppushing_filename)
	CLOG_INFO "文件请求:来自 \"${client}\" 的 \"${filename}\" 文件"

	# 0. 最简单访问控制
	#################################
	if [ "${IS_LOGIN}" != "${ACCESS_CTRL_AUTH}" ] ; then
		CLOG_WARN "拒绝文件传输"
		show_need_login
		bt_at_send 'AT+BTOPPACPT=0'
		bt_at_get_rsp "OK" ;ret=$?
		if [ $ret -ne ${COMMAND_RET_OK} ] ; then
			CLOG_ERR "拒接文件失败"
			return 100
		fi
		g_preStatus="${g_curStatus}";
		return 0
	fi

	# 1. 保存文件到模块文件系统.内部系统默认路径为 "C:\User\BtReceived\"
	#################################
	CLOG_INFO "发送接收文件响应"
	bt_at_send 'AT+BTOPPACPT=1'
	bt_at_get_rsp "OK" ;ret=$?
	if [ $ret -ne ${COMMAND_RET_OK} ] ; then
		CLOG_ERR "发送接收文件响应失败"
		return 100
	fi

	# 这两步可能一次性快速的返回,
	isRet "+BTOPPPUSH: 1" | ret=$? # 一次就读取到2不数据
	if [ ${ret} -ne ${COMMAND_RET_OK} ] ; then
		bt_at_get_rsp "+BTOPPPUSH:" 1000 ;ret=$? # 或者分到第二部
		if [ $ret -ne ${COMMAND_RET_OK} ] ; then
			CLOG_ERR "接收文件失败"
			return 101
		fi
	fi
	CLOG_INFO "接收文件成功"

	# 2. 从模块转移到linux文件系统
	#################################
	# 文件保存好后从模块文件系统转移到Linux文件系统.使用模块at指令
	# 参照 SIM800系列_FS_应用文档_V1.01.pdf

	filefullname="${SIM800_FILESYSTEM_BT_DEFAULT_DIR}${filename}"
	# 获得文件大小
	CLOG_INFO "- 文件名: ${filename}"
	CLOG_INFO "- 全路径: ${filefullname}"
	get_file_size "${filename}" ;ret=$?
	if [ ${ret} -ne ${COMMAND_RET_OK} ] ; then
		CLOG_ERR "读取文件大小失败"
		return 103
	fi
	size=$(echo "${ret_string}" | grep -e "+FSFLSIZE:" | sed 's/+FSFLSIZE: //' | grep -o '[0-9.]*')
	CLOG_INFO "获得文件大小  size=${size} 字节"

	mdelay 100
	fs_bt2sys "${filename}" "${size}"

	# 3. 删除模块中文件
	#########################
	if [ ${isDeleteReciveFileInBlueToothMod} == 1 ] ; then
		bt_at_send "AT+FSDEL=${filefullname}"
		bt_at_get_rsp "OK\|ERROR";ret=$?
		if [ ${ret} -ne 0 ] ; then
			CLOG_ERR "删除模块内文件失败"
			return 107
		fi
		CLOG_INFO "删除模块内文件成功"
	fi

	# 有连接的客户端控制台,没有控制台就不发送信息了
	if ( isNowHasClientConnected ) ; then
		send_to_client_tty "文件 ${filename} 接收成功"
	fi
	g_preStatus="${g_curStatus}";
	return 0;
}
# 状态机动作:空闲
DO_idle () {
	if [ ${idle_mark} == ${IDLE_MARK1} ] ; then
		idle_mark=${IDLE_MARK2}
	elif [ ${idle_mark} == ${IDLE_MARK2} ] ; then
		idle_mark=${IDLE_MARK3}
	elif [ ${idle_mark} == ${IDLE_MARK3} ] ; then
		idle_mark=${IDLE_MARK4}
	elif [ ${idle_mark} == ${IDLE_MARK4} ] ; then
		idle_mark=${IDLE_MARK1}
	fi
	echo -en "${idle_mark}\b"
	#echo -en " $(show_status_when_idle)"
	#show_status_when_idle
	return 0;
}
# 状态机动作:接受配对请求
DO_accept_pair () {
	echo -e "${CL_GREEN}监听到配对请求${CL_END}"
	echo -e "${CL_GREEN}接受配对请求${CL_END}"
	bt_at_send 'AT+BTPAIR=1,1'
	bt_at_get_rsp "OK" 20;ret=$?
	if [ $ret -eq 0 ] ; then
		CLOG_INFO "发送接受响应成功"
	else
		CLOG_ERR "发送接受响应失败"
		return 100;
	fi
	# 等待客户端响应
	bt_at_get_rsp "^+BTPAIR:*" 1000 ;ret=$?
	if [ $ret -eq 0 ] ; then
		CLOG_INFO "客户端响应成功" # 可能是确认配对或者拒接配对
		if ( echo "${g_curStatus}" | grep -q -e "^+BTPAIR: 0" ) ; then
			CLOG_WARN "客户端拒接配对"
		fi
	else
		CLOG_ERR "客户端响应失败"
		return 101;
	fi
	g_preStatus="${g_curStatus}";
	return 0;
}

# 状态机动作:获取客户端信息
DO_get_client_info () {
	#if [ $ret -ne ${COMMAND_RET_OK} ] ; then
	#	CLOG_ERR "获取客户端信息失败"
	#	return 100;
	#fi
	CLIENT_ID=$(get_connect_client_id ${ret_string})
	CLIENT_NAME=$(get_connect_client_name ${ret_string})
	echo -e "客户端[${CL_GREEN}${CLIENT_ID}:${CLIENT_NAME}${CL_END}]连接成功"
	g_preStatus="${g_curStatus}";
	return 0
}
# 状态机动作:接受连接请求,给予允许连接响应
DO_accept_connect () {
	client_mac=$(get_client_mac ${ret_string})
	echo -e "发现${CL_GREEN}${client_mac}${CL_END}发起的连接请求"
	echo -e "${CL_GREEN}接受连接${CL_END}"
	bt_at_send 'AT+BTACPT=1'
	bt_at_get_rsp "OK" 500;ret=$?
	if [ $ret -ne ${COMMAND_RET_OK} ] ; then
		echo -e "${CL_WARN}建立连接失败,再试一次${CL_END}"
		bt_at_send 'AT+BTACPT=1'
		bt_at_get_rsp "OK" 500;ret=$?
		if [ $ret -ne ${COMMAND_RET_OK} ] ; then
			echo -e "${CL_ERR}建立连接失败.${CL_END}"
			return 100;
		fi
	fi

	return 0;
}

# 清理一下蓝牙接收区
clean_bt_recv_folder () {
	CLOG_INFO "清理蓝牙接收文件夹"
	# ls一下接收目录
	local qty=0 #文件数量
	local i=0
	bt_at_send "AT+FSLS=${SIM800_FILESYSTEM_BT_DEFAULT_DIR}"
	bt_at_get_rsp "OK" 100 1;ret=$?
	if [ $ret -ne ${COMMAND_RET_OK} ] ; then
		CLOG_ERR "获得文件列表失败"
		return 100;
	fi
	local filelist_back="$(echo "${ret_string}"|sed '1d')"
	qty=$(echo "${filelist_back}" | wc -l)
	qty=$(expr $qty - 2) # 最后2行是空行和ok
	#CLOG_INFO "${qty}\"${filelist_back}\""
	while [ ${i} -lt ${qty} ] ; do
		i=$(expr $i + 1) # 1~文件数
		filename="$(echo "${filelist_back}" | sed -n ${i}p)"
		CLOG_INFO "文件: ${filename}"
		filefullname="${SIM800_FILESYSTEM_BT_DEFAULT_DIR}${filename}"
		bt_at_send "AT+FSDEL=${filefullname}"
		bt_at_get_rsp "ERROR\ OK" 20;ret=$?
		if [ ${ret} -eq 0 ] ; then
			CLOG_INFO "删除模块内文件成功 $i : ${filefullname}"
		else
			CLOG_ERR "删除模块内文件失败 $i : ${filefullname}"
			nop;
		fi
	done
	return 0;
}
# 返回是否是蓝牙定义指令.
# 第一个字符是否是 @
isBlueToothCommand () {
	local command=$1
	if [ "${command:0:1}" == "@" ] ; then
		return ${IS_BTCOMMAND}
	fi
}

# 蓝牙模块脚本实现指令处理
# 输入 $1 是以空格分割的命令.
# 例如: @li root password
btcommand_loop () {
	local in="$1"
	local arg0=$(echo "${in}" | awk '{print $1}')
	# 内部指令路由: 登陆 login
	if [ "${arg0}" == "@li" ] ; then
		if ( isLogin ) ; then
			#send_to_client_tty "请勿重复登陆"
			send_to_client_tty "Do not repeat the login."
		else
			local user=$(echo "${in}" | awk '{print $2}')
			local pass=$(echo "${in}" | awk '{print $3}')
			if [ "${user}" == "root" ] && [ "${pass}" == "LY9x25" ] ;then
				IS_LOGIN="1"
				CLOG_INFO "== 登录 =="
				send_to_client_tty "登陆成功"
			else
				CLOG_WARN "用户名或密码错误"
				send_to_client_tty "用户名或密码错误"
			fi
		fi
	# 登出 logout
	elif  [ "${arg0}" == "@lo" ] ; then
		if ( isLogin ) ; then
			IS_LOGIN="0"
			CLOG_INFO "== 登出 =="
			send_to_client_tty "登出"
		fi
	# 上传文件,客户端发起取文件请求. get,需要参数1:linux文件系统文件全路径
	elif  [ "${arg0}" == "@get" ] ; then
		if ( isLogin ) ; then
			local who=$(echo "${in}" | awk '{print $2}')
			local file=$(echo "${in}" | awk '{print $3}')
			if [ "${file}" == "" ] ;then # 如果只有一个参数,则自动求得客户端id
				who="1" # 默认就用1吧,简化程序,必要就@lsp获取吧
				file=$(echo "${in}" | awk '{print $2}')
			fi
			command_send_file "${who}" "${file}"
		else
			CLOG_ERR "未登陆"
			send_to_client_tty "未登陆,请使用@li <用户名> <密码>登陆"
		fi
	# unpair 清除配对信息
	elif  [ "${arg0}" == "@unp" ] ; then
		unpair;
	# 列举配对用户
	elif  [ "${arg0}" == "@lsp" ] ; then
		show_status;stat=${ret_string}
		send_to_client_tty "$(echo ${stat})"
		#echo "d:${ret_string}"
		#echo "d:${stat}"
		#echo ${stat}|grep -e "P:*"
		#bt_at_send 'AT+BTSPPSEND'
		# 如果按照这个判断,显示的地方会增加^M^J>的头,不知道为什么,
		# 但是不以这个判断可能断在不确定的地方,所以先这样着.(多次运行也可以避免错误
		#bt_at_get_rsp ">" 200;ret=$?
		#bt_at_send "$(echo ${stat})"
		#bt_at_send '\032' # 发送完成Ctrl+D # \032 x1a ^Z
		#mdelay 100
		#bt_at_get_rsp "SEND OK" 2000;ret=$?
		#if [ $ret -ne ${COMMAND_RET_OK} ] ; then
		#	CLOG_ERR "发送失败"
		#	return 100;
		#fi
	# 非法的内部指令,不响应
	else
		CLOG_ERR "非法的内部指令 ${arg0}"
		send_to_client_tty "非法的内部指令 ${arg0}"
	fi

	g_preStatus="${g_curStatus}";
	return 0;
}
# 状态机动作:接受并执行命令
# 通过判断第一个字符是否是@分2类
# 1. @开头  : 蓝牙定义指令,本脚本处理
# 2. 非@开头: 直接传递给操作系统脚本(功能较强,小心使用)
DO_eval_command () {
	# 获得命令行
	in=$(echo "${ret_string}" | grep -e "+BTSPPDATA:" | sed 's/+BTSPPDATA: //' | awk -F, '{print $3}'| tr -d '\r')
	# 蓝牙定义指令,
	isBlueToothCommand "${in}";ret=$?
	if [ $ret -eq ${IS_BTCOMMAND} ] ; then
		CLOG_INFO "获得自定义bt命令:[${in}]"
		change_to_at_mode;
		bt_at_send 'AT+BTSPPCFG?';
		bt_at_get_rsp "OK" 200;ret=$?;
		btcommand_loop "${in}"
	else
		if ( isLogin ) ; then
			CLOG_INFO "获得数据:[${in}]"
			vhex=$(phex "${ret_string}");
			echo "${vhex}"
			vhex=$(phex "${in}");
			echo "${vhex}"
			change_to_at_mode;
			send_to_client_tty "$(${in} 2>&1)"
		else
			CLOG_ERR "未登陆"
			send_to_client_tty "未登陆,请使用@li <用户名> <密码>登陆"
		fi
	fi
	g_preStatus="${g_curStatus}";
	return 0;
}

# 给客户端的终端发送文字.
# $1 发送给客户端的字符串
send_to_client_tty() {
	output="$1";
	bt_at_send 'AT+BTSPPSEND'
	# 如果按照这个判断,显示的地方会增加^M^J>的头,不知道为什么,
	# 但是不以这个判断可能断在不确定的地方,所以先这样着.(多次运行也可以避免错误
	bt_at_get_rsp ">" 200;ret=$?
	bt_at_send "${output}\032"
	#mdelay 100
	#bt_at_send '\032' # 发送完成Ctrl+D # \032 x1a ^Z
	#echo -en "\032" > /tmp/ctrld
	#dd if="/tmp/ctrld" of="${BLUE_TOOTH_TTY_DEV}" bs=1

	bt_at_get_rsp "SEND OK" 200;ret=$?
	if [ $ret -ne ${COMMAND_RET_OK} ] ; then
		CLOG_ERR "发送失败"
		return 100;
	fi
}
# 这个操作似乎可以清理一下缓冲,否则可能下一条指令一定返回error
# 暂时不知道原理是什么,好像由客户端触发的指令后用一下这个比较好
nop() {
	bt_at_send 'AT'
	bt_at_get_rsp "OK" 200
}
change_to_at_mode() {
	bt_at_send 'AT'
	bt_at_get_rsp "OK" 200
}
# 状态机动作:登陆
DO_login () {
	return 0;
}
# 状态机动作:登出
DO_logout () {
	return 0;
}

# 访问控制,判断是否登陆
# 登陆返回1,未登陆返回0
isLogin () {
	if [ "${IS_LOGIN}" != "${ACCESS_CTRL_AUTH}" ] ; then
		return 1;
	else
		return 0;
	fi
}
get_opppushing_clientname () {
	echo "${ret_string}" \
	| grep -e "+BTOPPPUSHING:" \
	| sed 's/+BTOPPPUSHING: //' \
	| awk -F, '{print $1}' \
	| sed -e 's/^"//'  -e 's/"$//'
}

get_file_size () {
	bt_at_send "AT+FSFLSIZE=${SIM800_FILESYSTEM_BT_DEFAULT_DIR}${1}"
	#bt_at_send 'AT+FSFLSIZE=C:\\User\\BtReceived\\' # just for debug
	bt_at_get_rsp "OK" 20; ret=$?

	if [ $ret -ne ${AT_RET_OK} ] ; then
		CLOG_ERR "获取文件大小失败"
		return 100
	fi

	if ( echo ${ret_string} | grep -q -e "OK" ) ; then
		return 0
	else
		return 200
	fi
}

# 设置蓝牙模块显示的名称 ref1:2.1
# $1 名称utf8编码 名字最长只支持 18 个字符,以 UTF-8 编码格式显示
sethostname() {
	bt_at_send "AT+BTHOST=${1}"
	bt_at_get_rsp "OK" 20; ret=$?
}

# 获得客户端上传的文件名
get_opppushing_filename () {
	echo "${ret_string}" | sed 's/\r$//'| tr -d '\r' | grep -e "+BTOPPPUSHING:" | sed 's/+BTOPPPUSHING: //'| awk -F, '{print $2}'|sed -e 's/^"//'  -e 's/"$//'
	#echo "运行一次"  >> /data/1.log
}

# 以16进制形式打印参数字符串
# @param $1 待转义成16进制的字符串
phex () {
	echo -n "$@" |od -A n -t x1
}

# 当前是否有客户端连接着?
# 当前有连着的话就可以跳过等待配对和连接过程了
# @retval HAS_CLIENT_CONNECTED	有
# @retval NO_CLIENT_CONNECTED	没有或者错误等
isNowHasClientConnected () {
	show_status
	echo -e ${ret_string} | grep -q -e "C:" ; ret=$?
	if [ $ret -eq 0 ] ; then
		echo -e "${CL_GREEN} == 以下客户端已连接 ==${CL_END}"
		echo -e ${ret_string} | grep -e "C:"
		return ${HAS_CLIENT_CONNECTED}
	fi
	return ${NO_CLIENT_CONNECTED}
}

# 调试时常用,删除模块中已经存储的配对信息
# 0 表示删除所有的. ref1:2.5
unpair() {
	bt_at_send "AT+BTUNPAIR=0"
	bt_at_get_rsp "OK" 20; ret=$?
}

# 主动发起搜索
scan() {
	echo -e "${CL_GREEN}主动发起扫描...${CL_END}"
	bt_at_send 'AT+BTSCAN=1,20'
	bt_at_get_rsp "^+BTSCAN: 1$";ret=$?
}

get_client_mac () {
	echo "${@}" | grep -e "+BTCONNECTING:" | sed 's/+BTCONNECTING: //'| awk -F, '{print $1}'
}

get_connect_client_name () {
	echo "${@}" | grep -e "+BTCONNECT:" | sed 's/+BTCONNECT: //'| awk -F, '{print $2}'
}

get_connect_client_id () {
	echo "${@}" | grep -e "+BTCONNECT:" | sed 's/+BTCONNECT: //'| awk -F, '{print $1}'|grep -e '[0-9]*' -o
}
# 以mac地址获得当前spp方式连接的客户端的pair序号
# $1 mac地址
get_spp_client_by_mac() {
	local mac="$1"
	echo "${lists}" |grep "${mac}"|grep "P:" |sed 's/P://'| awk -F, '{print $1}'|grep -e '[0-9]*' -o
}

# 返回以spp方式连接的客户端的mac地址
get_spp_mac() {
	echo "${lists}" |grep "SPP"|sed 's/C://' | awk -F, '{print $3}'|grep -e '[0-9a-z:]*'
}
# 启动蓝牙,文档说需要检测状态才能启动,只有在关的状态才可以启动蓝牙
# 如果已经启动蓝牙,则除了查询一下状态之外没有其他副作用.
start_bluetooth () {
	local wait_for_btstatus_timeout_counter=30
	i=0;
	while [ ${i} -lt ${wait_for_btstatus_timeout_counter} ] ; do
		i=$(expr $i + 1)
		CLOG_WARN "检查蓝牙启动状态... $i/${wait_for_btstatus_timeout_counter}"
		is_bluetooth_close ;ret=$?
		if [ ${ret} -eq 0 ] ; then
			CLOG_INFO "蓝牙未开启,正在启用..."
			nop;
			poweron
			#return 0; # 调试开关机用
		elif [ ${ret} -eq 1 ] ; then
			CLOG_INFO "蓝牙已开启"
			return 0;
		else
			CLOG_ERR "获得状态失败,模块未初始化? ret=${ret}"
			return 100;
		fi
		sleep 1
	done
	CLOG_ERR "获得状态失败,模块未初始化?,具体措施?手动初始化模块还是其他?"
	return 100;
}

# 开启蓝牙电源
poweron() {
	bt_at_send 'AT+BTPOWER=1'
	bt_at_get_rsp "OK" 1000;ret=$?
	if [ ${ret} -ne ${COMMAND_RET_OK} ] ; then
		CLOG_ERR "开启蓝牙失败 ${ret}"
		return 100
	fi
	return 0;
}
# 判断蓝牙是否关闭,只有关闭着才可以开启蓝牙.
# retval
# 	0 关闭着
# 	1 没关闭(开着,空闲)
# 	100 错误
# 	200 取得状态错误
is_bluetooth_close () {
	bt_at_send 'AT+BTSTATUS?'
	bt_at_get_rsp "OK" 100;ret=$?
	lists=$(echo  ${ret_string} | sed -e 's/\x0D/\x0A/g')
	# get_spp_mac
	# get_spp_client_by_mac "$(get_spp_mac)"
	#debug_print "******** ret="${ret}
	#debug_print "******** ret_string='"${ret_string}"'"
	if [ ${ret} -ne ${COMMAND_RET_OK} ] ; then
		CLOG_ERR "ret_string=[${ret_string}]"
		ret=100;
	else
		if ( isRet "+BTSTATUS:" ) ; then
			if ( isRet "+BTSTATUS:\ 0" ) ; then
				ret=0;
			elif ( isRet "+BTSTATUS:\ 5" ) ; then
				ret=1;
			else
				CLOG_WARN "意外状态 ret_string=[${ret_string}]"
				ret=50
			fi
		else
			CLOG_ERR "没有找到 BTSTATUS ret_string=[${ret_string}]"
			ret=200;
		fi
	fi
	return "${ret}"
}

# 显示当前状态
show_status() {
	bt_at_send 'AT+BTSTATUS?'
	bt_at_get_rsp 'OK' 200; ret=$?
	if [ $ret -ne ${COMMAND_RET_OK} ] ; then
		CLOG_WARN "取得状态失败,再试一次"
		bt_at_send 'AT+BTSTATUS?'
		bt_at_get_rsp 'OK' 200; ret=$?
		if [ $ret -ne ${COMMAND_RET_OK} ] ; then
			CLOG_ERR "取得状态失败"
			return 100;
		fi
	fi
	#echo -e "${CL_PURP}**** 当前状态 ****${CL_END}"
	#echo -e "${CL_PURP}${ret_string}${CL_END}"
	g_preStatus="${g_curStatus}";
	return 0
}
# 空闲时显示的信息
show_status_when_idle() {
	bt_at_send_without_log 'AT+BTSTATUS?'
	bt_at_get_rsp 'OK' 200 0 0; ret=$?
	#if [ $ret -ne ${COMMAND_RET_OK} ] ; then
		#CLOG_ERR "."
		#return 100;
	#fi
	echo "${g_curStatus}" | grep -e "+BTSTATUS:*"
	#echo -e "${CL_PURP}**** 当前状态 ****${CL_END}"
	#echo -e "${CL_PURP}${ret_string}${CL_END}"
	g_preStatus="${g_curStatus}";
	return 0
}

debug_print() {
	echo -e "$@"
}
# 发送at命令,需要延时发送,等到接收指令运行后才开始
bt_at_send() {
	echo -n ''> "${BLUE_TOOTH_BUFF}"
	#echo -en "$@\r\n" > ${BLUE_TOOTH_TTY_DEV}
	CLOG_DEBUG_SEND "${@}"
	echo -e "${@}" > ${BLUE_TOOTH_TTY_DEV}
}
bt_at_send_without_log() {
	echo -n ''> "${BLUE_TOOTH_BUFF}"
	#echo -en "$@\r\n" > ${BLUE_TOOTH_TTY_DEV}
	#CLOG_DEBUG_SEND "${@}"
	echo -e "${@}" > ${BLUE_TOOTH_TTY_DEV}
}

# 设置蓝牙串口属性
set_dev_attr() {
	stty -F ${BLUE_TOOTH_TTY_DEV} raw 115200 cs8 -cstopb -parenb -icanon min 1 time 1
}

# 对一行结果进行正则表达式计算
# @param[in] $1 grep 正则表达式语句. 对每一行进行正则运算
# @param[in] $2[可选] 覆盖默认的超时时间 默认:wait_for_response_default_timeout_second
# @param[in] $3[可选] 保存成为文件(下载文件时用.默认不保存)
#
# @revtval AT_RET_OK 正确
# @revtval AT_RET_NOT_FIND 超时没找到
#
# 调用格式:
# 	bt_at_get_rsp <结束正则表达式>
# 	bt_at_get_rsp <结束正则表达式> <最大超时百毫秒数>
# 	bt_at_get_rsp <结束正则表达式> <最大超时百毫秒数> <保存成为文件:1>
# 	bt_at_get_rsp <结束正则表达式> <最大超时百毫秒数> <保存成为文件:1> <显示log(默认显示)>
bt_at_get_rsp () {
	local ret=${AT_RET_NOT_FIND};
	local i=0;
	#echo "参数1=$@"
	timeout=${wait_for_response_default_timeout_second}
	if [ $# -ge 2 ] ;then
		timeout=$2
	fi
	savefile=0;
	if [ $# -ge 3 ] ;then
		savefile=$3
	fi
	showlog=1
	if [ $# -ge 4 ] ;then
		showlog=$4
	fi
	while [ "$i" -lt "${timeout}" ]; do
		# 查找表达式
		grep -q -e "${1}" "${BLUE_TOOTH_BUFF}"; ret=$?
		# 如果查找到
		if [ $ret -eq 0 ] ; then
			if [ "$savefile" -eq 1 ] ; then
				cp "${BLUE_TOOTH_BUFF}" "${BLUE_TOOTH_BUFF}.tmpsavefile"
			fi
			#echo -e "******${CL_PURP}读取完成${CL_END}*****"
			ret_string="$(cat ${BLUE_TOOTH_BUFF})"
			g_curStatus="$(cat ${BLUE_TOOTH_BUFF})"
			# echo ${ret_string}|hexdump -ve '1/1 "%.2x"'
			if [ ${showlog} -eq 1 ] ; then
				CLOG_DEBUG_RECV "${ret_string}"
			fi
			# vhex=`phex ${ret_string}`;
			# echo "${vhex}"
			ret=${AT_RET_OK};break;
		fi
		# 继续查找,直到超时
		mdelay 100;
		i=$((i + 1))
	done
	# 查找结束
	echo -n ''> "${BLUE_TOOTH_BUFF}"
	# ret=${AT_RET_NOT_FIND}
	return "${ret}";
}

#延时毫秒
# 参数1 毫秒数
mdelay () {
	#usleep $(expr "$1" \* 1000);
	sleep $(expr "$1" / 1000);
}

CLOG_INFO () {
	echo -en "${CL_GREEN}"
	echo -n "${@}"
	echo -e "${CL_END}"
}

CLOG_WARN () {
	echo -en "${CL_YELLOW}"
	echo -n "${@}"
	echo -e "${CL_END}"
}

CLOG_ERR () {
	echo -en "${CL_RED}"
	echo -n "${@}"
	echo -e "${CL_END}"
}

CLOG_DEBUG_SEND () {
	echo -en "${CL_RED}->${CL_END} ${CL_CYAN}"
	echo -n "${@}"
	echo -e "${CL_END}"
	return 0;
}

CLOG_DEBUG_RECV () {
	echo -en "${CL_RED}<-${CL_END} ${CL_PURP}"
	echo -n "${@}"
	echo -e "${CL_END}"
	return 0;
}

#####################################
# 定义各种返回值宏.相对数字更容易看懂
#####################################
HAS_CLIENT_CONNECTED=0
NO_CLIENT_CONNECTED=100
AT_RET_NOT_FIND=100
AT_RET_OK=0
COMMAND_RET_OK=0
SAVE_MSG_TO_FILE=1
ACCESS_CTRL_AUTH="1"
ACCESS_CTRL_NOT_AUTH="0"

IS_BTCOMMAND=50
IS_NOT_BTCOMMAND=51
# 空闲标识
idle_mark='-'
IDLE_MARK1='-'
IDLE_MARK2='\\'
IDLE_MARK3='|'
IDLE_MARK4='/'

# 发送文件流程: 空闲->等待客户端响应->空闲
FILE_DIA="IDLE"

# 接收文件流程: 空闲->

# tty color define
CL_RED="\033[31m"
CL_GREEN="\033[32m"
CL_YELLOW="\033[33m"
CL_BLUE="\033[34m"
CL_PURP="\033[35m"
CL_CYAN="\033[36m"
CL_END="\033[0m"
## start script


main "$@"

# 参考资料
# 1. SIM800系列_BT_应用文档_V1.05.pdf
```
