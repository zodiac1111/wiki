# RubyGems 镜像 - 淘宝网

http://ruby.taobao.org/

# 为什么有这个？

由于国内网络原因（你懂的），导致 rubygems.org 存放在 Amazon S3 上面的资源文件间歇性连接失败。所以你会与遇到 `gem install rac`k 或 `bundle install` 的时候半天没有响应，具体可以用 `gem install rails -V` 来查看执行过程。

# 如何使用？
```
$ gem sources --remove https://rubygems.org/
#也可能是 gem sources --remove http://rubygems.org/
$ gem sources -a http://ruby.taobao.org/
$ gem sources -l
*** CURRENT SOURCES ***

http://ruby.taobao.org
# 请确保只有 ruby.taobao.org
$ gem install rails
```

# 如果你是用 Bundle (Rails 项目)

```
source 'http://ruby.taobao.org/'
gem 'rails', '3.2.12'
...
```

# 更多

请参阅来源 http://ruby.taobao.org/