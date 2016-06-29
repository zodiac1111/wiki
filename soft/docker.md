# host和container数据共享

[参考](https://www.digitalocean.com/community/tutorials/how-to-work-with-docker-data-volumes-on-ubuntu-14-04)

```
user@host: mkdir /tmp/m_in_host
user@host: sudo docker run -t -i -v /tmp/m_in_host:/tmp/m_in_container unikernel/mirage
```
`unikernel/mirage` 是image名称

在容器中查看

```
opam@container:/src$ ls /tmp/
m_in_container
```

## 自用例子

```
sudo docker run -t -i -v ~/tmp/m_in_host:/tmp/m_in_container zodiac1111/mirage
```


# host和容器复制文件

[参考](http://stackoverflow.com/questions/22049212/docker-copy-file-from-container-to-host)

```
docker cp <containerId>:/file/path/within/container /host/path/target
```


jenkins blueocean 

https://hub.docker.com/r/jenkinsci/blueocean/


# dockerfile

简单的看得懂的例子,可以用来理解dockerfile. 南瓜日语自然语言处理 句子

https://github.com/skozawa/docker-cabocha-unidic/blob/master/Dockerfile

# 更新

```
sudo docker pull blueocean/blueocean
```

# 运行

```
sudo docker run -p 8888:8080 blueocean/blueocean
```
其中`-p 8888:8080`是端口映射,形式是` -p [host端口:docker容器端口 [host端口:docker容器端口]]`, 可以映射多个端口.
