# linuxnote  

# 操作文件和目录
- 通配符： [wildchar 通配符]  
- 创建目录：[mkdir make directory 创建目录]
- 复制：[cp copy]
- 移动或重命名：[mv move 移动或重命名]
- 删除：[rm remove 删除]  


## wildchar 通配符

- '*' : 0,1,more char
- ? : 1 char
- [character] : one char in set
- [!character] : not one char in set
- [[:class:]] : one char in class

- [:digit:] : number
- [:lower:] : lower char
- [:upper:] : upper char
- [:alnum:] : num + alpha
- [:alpha:] : low and upper


![](linuxnote_files/1.jpg)

## mkdir make directory 创建目录

- mkdir dir1
- mkdir dir1 dir2 dir3  

![](linuxnote_files/2.jpg)

## cp copy

- cp item1 item2  #把单个的文件或目录item1复制成item2
- cp item... directory #把多个文件复制到一个文件夹中
- options
		-a --archive #复制文件时把原属性（所有权，权限）也复制上
		-i --interactive #交互式的信息确认
		-r --recursive #递归，复制文件时把原文件下所有子文件也复制上
		-u --update #只复制更新内容
		-v --verbose #复制时提供详细信息
		
				
![](linuxnote_files/3.jpg)
![](linuxnote_files/4.jpg)

## mv move 移动或重命名

- mv item1 item2
- mv item... directory

![](linuxnote_files/5.jpg)

## rm remove 删除
- rm item...
- option
		- f --force

注：linux没有还原命令

![](linuxnote_files/6.jpg)

## ln link

- ln file link #创建硬链接
- ln -s item link #创建符号链接（软链接）

硬链接：一开始就有，文件与文件源数据（属性）有硬链接

缺点：不能指向目录；不能链接两个设备上的文件；不能看出两个文件的硬链接关系

符号链接：为了克服硬链接缺点而创建，比windows快捷方式还要早出。
删除符号链接文件不改变源文件

应用于版本更新，名字改变只需改软链接就行
foo ->foo2.1
foo ->foo2.3

硬链接：  
![](linuxnote_files/7.jpg)
软链接：  
![](linuxnote_files/8.jpg)
删除源文件后的软链接：  
![](linuxnote_files/9.jpg)  
符号链接可以对目录作链接：  
![](linuxnote_files/10.jpg)



# 操作命令

## 命令类型
1. execute binary #可执行的二进制程序，显示颜色是绿色
2. buildin bash #shell 内嵌
3. shell function
4. alias #别名

## 了解命令类型
- type command #返回cmd的类型

![](linuxnote_files/11.jpg)


- which command #返回cmd的文件位置

![](linuxnote_files/12.jpg)  

## 查看帮助  
- help command #返回shell内嵌命令的帮助信息
- man command #manual 帮助手册
- 查看多页文档
- g 到页头 G 到页顶 
- ctrl+u 向上翻页 ctrl+d 向下翻页
- q 退出

## 关键字搜索命令全称  

- apropos keyword #搜索关键字寻找命令
- man -k keyword #功能同上

![](linuxnote_files/13.jpg)
![](linuxnote_files/14.jpg)

## 命令简介

- whatis cmd #比help简单，多为一行文字

![](linuxnote_files/15.jpg)

- info keyword #树形结构，有超链接，比manual易懂
- 操作
	- n:next node
	- p:preview node
	- q:quit
	- u:up
	- enter:jump to link
	- space:pagedown


## alias

- alias name='cmd string'

![](linuxnote_files/16.jpg)

- alias #list alias cmd
- unalias name #remove alias cmd



# I/O重定向

## redirection
- stdout standard out device #标准的输出重定向到文件中
- stdrr standard error device 
- stdin standard input device

### redirect standard output #重新编辑输出，一般输出到屏幕
- command > filename #用大于号编辑输出路径到一个文件
- 标准输出重定向生成的文件有字节，但错误命令重定向生成空文件
- 技巧：利用错误命令重定向可以清空文件内容，得到空文件

![](linuxnote_files/17.jpg)

- command >> filename #输出追加

![](linuxnote_files/18.jpg)

![](linuxnote_files/19.jpg)

### redirect standard error
- 在大于号前加数字2
- 0:studio
- 1:stdout
- 2:stderr


![](linuxnote_files/21.jpg)

![](linuxnote_files/20.jpg)

### redirect stdout and stderr #同时重定向标准输出和错误到一个文件
- ls -l . /bin/usr > ls-output.txt 2>&1
- 先错误输出然后正确输出
- ls -l . /bin/usr &>1 ls-output.txt 

![](linuxnote_files/23.jpg)

![](linuxnote_files/22.jpg)

### useless message

- /dev/null #相当于垃圾回收站
- ls -l /bin/usr 2> /dev/null


![](linuxnote_files/24.jpg)


### redirect stdin

- cat file

- cmd < file

![](linuxnote_files/25.jpg)
- ctrl+d  #退出cat

### pipe line 
- 管道线可以把多个命令组合起来使用
- cmd | cmd #一个命令的输出作为另一个命令的输入

![](linuxnote_files/26.jpg)

### filter 
- 在管道线中加入过滤器，把输入数据经过转换再输入下一个命令
- sort #排序
- unique #去重复
- wc #word count
- grep pattern [file]
	- grep -i ignore caption #忽略大小写
- head -n 5 #打印前5行
- tail -n 5

- tee stdin/stdout #中间插入，得到中间结果


	ls -l /usr/bin | sort | uniq | wc -l
	ls -l /usr/bin | sort | uniq | grep zip
	ls -l /usr/bin | sort | uniq | head -5
	ls -l /usr/bin | sort| tee ls.txt | uniq | grep zip  
	
![](linuxnote_files/27.jpg)




## expansion

- character expansion 
	- echo [string]
	- echo * #打印当前目录的所有东西，*为通配符，系统把符号展开了
	- 
- pathname expansion
	- echo *.txt

	- echo .* 
	- echo .[!.]*

- tilde expansion #波浪线展开
	- echo ~

- arithmetic expansion
		
		$((expression))
		支持 * - /  % **
		不支持小数
		echo $(((2*6)**3))

![](linuxnote_files/28.jpg)

- brace expansion #花括号展开
	- echo Front-{A,B,C}-End
	- echo Num_{1,2,3}
	- echo {A..Z}
	- echo a{A{1,2},B{3.4}}b
	- mkdir {2010..2019}-{1..12} #创建月份文件

![](linuxnote_files/29.jpg)

- parameter expansion #变量展开
	- echo $USER
	- printenv | less

![](linuxnote_files/30.jpg)



	














