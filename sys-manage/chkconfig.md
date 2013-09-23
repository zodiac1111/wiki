#chkconfig添加自定义服务

* 来源 http://os.51cto.com/art/200805/75144.htm

1、init.d目录下都为可执行程序，他们其实是服务脚本，按照一定格式编写，Linux 在启动时会自动执行，类似Windows下的服务

2、用root帐号登录，vi /etc/rc.d/init.d/mystart，追加如下内容：

```bash
#!/bin/bash
#chkconfig:2345 80 05 #--指定在哪几个级别执行，0一般指关机，6指的是重启，其他为正常启动。80为启动的优先级，05为关闭的优先机
#description:mystart service
RETVAL=0

start(){ #--启动服务的入口函数
    echo -n "mystart serive ..."
    cd ~
}

stop(){ #--关闭服务的入口函数
    echo "mystart service is stoped..."
}

case $1 in #--使用case，可以进行交互式操作
  start)
    start
  ;;
  stop)
    stop
  ;;
esac
exit $RETVAL
```
3、运行chmod +r /etc/rc.d/init.d/mystart,使之可直接执行

4、运行chkconfig --add mystart,把该服务添加到配置当中

5、运行chkconfig --list mystart,可以查看该服务进程的状态