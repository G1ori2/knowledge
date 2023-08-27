# 内核初始化





 # 常用指令

## 1 linux三剑客

- `cut`

  - 用指定的规则来切分文本

    举个🌰

    ```shell
    cut -d':' -f1,2,3 passwd | grep root
    ```

    ![image-20230624162129207](/Users/xiongyiru/Library/Application Support/typora-user-images/image-20230624162129207.png)

    顾名思义则是，选取`passwd`文件中，root开头的，` :`间隔的第1，2，3个词

- `sort`

  - 对文本进行字典序排序

    举个🌰

    ```shell
    sort -t ':' -k3 passwd
    ```

    在passwd文件中，根据“：”分隔符，对第三个字符进行字典序排序

  - `-r`逆序，`-n`按照数字大小

- `wc`

  - 统计单词的数量
  - `-l` line
  - `-w` word
  - `-c` char

### 1.1 剑客一号 grep

- 对文本进行搜索
- 同时搜索多个文件   ` grep target txt1 txt2 ..`
  - `-n `显示匹配的行号
  - `-nvi `显示不皮皮恶毒忽略大小写
  - `grep -E "[1-9]+" passed --color=auto` 使用正则表达式

### 1.2 剑客二号 sed

- 从管道中读取一行处理一行 ==》 压力较小

- 对文本实现增删改查

- 具体参考https://www.cnblogs.com/chensiqiqi/p/6382080.html

- 行的选择

  - 10 第十行
  - m,n    第m行到第n行
  - m,+n    第m行到第m+n行
  - m,$    第m行到最后一行
  - /school/    匹配到school的行
  - /u1/,/u4/  从匹配u1到匹配u4

- 举🌰

  ` sed [-i] '1,10 everything's gonna be ok' passwd`

  **<font color=red>注意！</font>**

  `-i`才是直接修改到磁盘，如果不加那么则只是在终端上进行修改

### 1.3 剑客三号 swk

- swk是一门语言
- 具体参考https://www.cnblogs.com/chensiqiqi/p/6228006.html

# 脚本

## 1 脚本生效

- 输入脚本和绝对路径或者相对路径
  - `/root/helloworld.sh/`
  - `./helloworld.sh`
  - 执行的权限⚠️得是可执行文件`x`
- bash或者sh + 脚本
  - `sh helloworld.sh`
  - 当脚本没有`x`权限的时候，root和文件所有者可以通过该方式正常执行
- 在脚本的路径前再加`.`或者`source`
  - ` source helloworld.sh`	

<font color=red>上述三者区别</font>

- 前两种是额外开了一个新的bash【进程】，不同的bash中的变量是无法共享的

- `source`是在当前进程

  ==》<font color=blue>比如，当我修改bash脚本内容的时候，source生效，而前两者不生效，需要`import`（将当前进程的变量传给子进程，<u>修改`profile`都用此方法</u>）来实现修改</font>



