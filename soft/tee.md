# 管道的各种使用

# 多个输入到tee入口

    ./a.out 2>&1 | tee output


# stderr和stdout一起到出口

[参考](http://www.cyberciti.biz/faq/redirecting-stderr-to-stdout/)

Redirecting the standard error (stderr) and stdout to file
Use the following syntax:

  $ command-name &>file

OR

  $ command > file-name 2>&1

Another useful example:

  # find /usr/home -name .profile 2>&1 | more