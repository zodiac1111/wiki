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
