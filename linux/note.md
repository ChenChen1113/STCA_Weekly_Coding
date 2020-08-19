### 历史命令
历史命令存放的位置：~/.bash_history
history     (查看历史命令)    
history -c  (清空历史命令)
history -w  (将shell中的历史命令写入historyfile)
history -r  (将historyfile中的命令读入shell缓存)
!n  (重复执行第n条命令，history命令输出会有条数)
!!  (重复执行上一条命令)
!str    (重复执行上一条str开头的命令)

### 命令别名
alias ls = 'sudo rm -rf *' (指定别名)
alias   (查看自己设置的所有别名)
unalias xxxx    (删除别名)

### 常用快捷键
ctrl+l  (清屏)
ctrl+u  (清空/剪切光标之前的命令)
ctrl+k  (删除/剪切光标之后的命令)
ctrl+y  (粘贴)
ctrl+r  (在history中搜索)
ctrl+d  (退出当前终端)
ctrl+s  (暂停屏幕输出)
ctrl+q  (回复屏幕输出)

### 重定向
cmd > file  (输出到文件，覆盖)
cmd >> file     (输出到文件，追加)
cmd 2> file (输出错误到文件，覆盖)
cmd 2>> file (输出错误到文件，追加)
cmd > file 2>&1 或  cmd &> file (同时保存正确和错误的输出到一个文件，覆盖)
cmd >> file 2>&1  或  cmd &>> file (同时保存正确和错误的输出到一个文件，追加)
cmd >> file1 2>> file2  (正确输出保存到file1，错误输出保存到file2)

### 输入重定向
wc < file   (统计file里有多少行、多少单词、多少字符)

### 多命令顺序执行
cmd1;cmd2   (顺序执行，两命令无关)
cmd1&&cmd2  (命令1执行正确后执行命令2)  如 ./configure && make && make install
cmd1||cmd2  (1正确就不执行2了，1错误才执行2)

### 管道
cmd1 | cmd2 (命令1的正确输出作为命令2的操作对象)

### 进程
ps aux (查看系统中所有进程，BSD操作系统格式)a前台进程，x后台进程，u用户
ps -le   (查看系统中所有进程，linux格式)
top (查看系统健康状态)
pstree  (查看进程树)
kill -1 xxxx    (重启进程)
kill -9 xxxx    (强制杀死进程)
df (disk free查看磁盘状态，包括刚删除的文件，当这个文件不被使用时才清空)
du (disk usage查看磁盘状态，通过计算已有文件的大小)

### 端口
lsof -i 查看端口情况
lsof -i : 8000 查看具体端口
netstat -ano|findstr "9097"
taskkill -F -PID xxxx

### 编辑文件
sed [option] 'command' filename

a\ 在当前行下面插入文本。
i\ 在当前行上面插入文本。
c\ 把选定的行改为新的文本。
d 删除，删除选择的行。
D 删除模板块的第一行。
s 替换指定字符
h 拷贝模板块的内容到内存中的缓冲区。
H 追加模板块的内容到内存中的缓冲区。
g 获得内存缓冲区的内容，并替代当前模板块中的文本。
G 获得内存缓冲区的内容，并追加到当前模板块文本的后面。
l 列表不能打印字符的清单。
n 读取下一个输入行，用下一个命令处理新的行而不是用第一个命令。
N 追加下一个输入行到模板块后面并在二者间嵌入一个新行，改变当前行号码。