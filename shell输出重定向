https://blog.csdn.net/liji_digital/article/details/127511288
https://www.runoob.com/linux/linux-comm-tee.html
https://www.cnblogs.com/benjaminpc/articles/16653136.html
https://tool.4xseo.com/article/82482.html

I/O重定向详解及应用实例

1、 基本概念（这是理解后面的知识的前提，请务必理解）

a、 I/O重定向通常与 FD有关，shell的FD通常为10个，即 0～9；

b、 常用FD有3个，为0（stdin，标准输入）、1（stdout，标准输出）、2（stderr，标准错误输出），默认与keyboard、monitor、monitor有关；

c、 用 < 来改变读进的数据信道(stdin)，使之从指定的档案读进；

d、 用 > 来改变送出的数据信道(stdout, stderr)，使之输出到指定的档案；

e、 0 是 < 的默认值，因此 < 与 0<是一样的；同理，> 与 1> 是一样的；

f、 在IO重定向 中，stdout 与 stderr 的管道会先准备好，才会从 stdin 读进资料；

g、 管道“|”(pipe line):上一个命令的 stdout 接到下一个命令的 stdin;

h、 tee 命令是在不影响原本 I/O 的情况下，将 stdout 复制一份到档案去;

i、 bash（ksh）执行命令的过程：分析命令－变量求值－命令替代（``和$( )）－重定向－通配符展开－确定路径－执行命令；

j、 ( ) 将 command group 置于 sub-shell 去执行，也称 nested sub-shell，它有一点非常重要的特性是：继承父shell的Standard input, output, and error plus any other open file descriptors。


k、 exec 命令：常用来替代当前 shell 并重新启动一个 shell，换句话说，并没有启动子 shell。使用这一命令时任何现有环境都将会被清除。exec 在对文件描述符进行操作的时候，也只有在这时，exec 不会覆盖你当前的 shell 环境。

2、 基本IO

cmd > file 把 stdout 重定向到 file 文件中；

cmd >> file 把 stdout 重定向到 file 文件中(追加)；

cmd 1> fiel 把 stdout 重定向到 file 文件中；

cmd > file 2>&1 把 stdout 和 stderr 一起重定向到 file 文件中；

cmd 2> file 把 stderr 重定向到 file 文件中；

cmd 2>> file 把 stderr 重定向到 file 文件中(追加)；

cmd >> file 2>&1 把 stderr 和 stderr 一起重定向到 file 文件中(追加)；

cmd < file >file2 cmd 命令以 file 文件作为 stdin，以 file2 文件作为 stdout；

cat <>file 以读写的方式打开 file；

cmd < file cmd 命令以 file 文件作为 stdin；

cmd << delimiter Here document，从 stdin 中读入，直至遇到 delimiter 分界符。

3、 进阶IO

>&n 使用系统调用 dup (2) 复制文件描述符 n 并把结果用作标准输出；

<&n 标准输入复制自文件描述符 n；

<&- 关闭标准输入（键盘）；

>&- 关闭标准输出；

n<&- 表示将 n 号输入关闭；

n>&- 表示将 n 号输出关闭；

上述所有形式都可以前导一个数字，此时建立的文件描述符由这个数字指定而不是缺省的 0 或 1。如：

... 2>file 运行一个命令并把错误输出(文件描述符 2)定向到 file。

... 2>&1 运行一个命令并把它的标准输出和输出合并。(严格的说是通过复制文件描述符 1 来建立文件描述符 2 ，但效果通常是合并了两个流。)

我 们对 2>&1详细说明一下 ：2>&1 也就是 FD2＝FD1 ，这里并不是说FD2 的值 等于FD1的值，因为 > 是改变送出的数据信道，也就是说把 FD2 的 “数据输出通道” 改为 FD1 的 “数据输出通道”。如果仅仅这样，这个改变好像没有什么作用，因为 FD2 的默认输出和 FD1的默认输出本来都是 monitor，一样的！ 但是，当 FD1 是其他文件，甚至是其他 FD 时，这个就具有特殊的用途了。请大家务必理解这一点。


exec 0exec 1>outfilename # 打开文件outfilename作为stdout。

exec 2>errfilename # 打开文件 errfilename作为 stderr。

exec 0<&- # 关闭 FD0。

exec 1>&- # 关闭 FD1。

exec 5>&- # 关闭 FD5。

问： 如果关闭了 FD0、FD1、FD2，其后果是什么？ 恢复 FD0、FD1、FD2与 关闭FD0、FD1、FD2 有什么区别？代码分别是什么？ 打开了FD3～FD9，我们用完之后，你觉得是将他们关闭还是恢复？

下面是提示（例子来源于CU一帖子，忘记出处，来日再补上）：

exec 6>&2 2>ver 
command >>dev/null & 
exec 2>&6 # 恢复 FD2
4、 简单举例

a、stdout和stderr都通过管道送给egrep了：

(ls you no 2>&1;ls yes 2>&1) 2>&1|egrep \* >file 
(ls you no 2>&1;ls yes 2>&1)|egrep \* >file 
(ls you no;ls yes) 2>&1|egrep \* >file
这个例子要注意的就是：

理 解 命令执行顺序 和 管道“|”：在命令执行前，先要进行重定向的处理，并将把 nested sub-shell 的stdout 接到 egrep 命令的 stdin。 nested sub-shell ，在 ( ) 中的两个命令加上（），可以看作一个命令。其 FD1 已经连接到“|”往egrep送了，当遇到 2>&1时，也就是FD2＝FD1，即FD2同FD1一样，往管道 “|”那边送。

b、 没有任何东西通过管道送给egrep，全部送往monitor。 (ls you no 2>&1;ls yes 2>&1) >&2|egrep \* >file。虽然在（）里面将 FD2转往FD1，但在（）外，遇到 >&2 ，结果所有的都送到monitor。 请理解：

(ls you no 2>&1) 1>&2|egrep \* >file ## 送到 monitor 
ls you no 2>&1 1>&2|egrep \* >file ## 送给 管道 “|” 
ls you no 1>&2 2>&1|egrep \* >file ## 送到 monitor
5、 中阶例子

条件： stderr通过管道送给egrep，正确消息仍然送给monitor（不变）

exec 4>&1;(ls you no 2>&1 1>&4 4>&-;ls yes 2>&1 1>
&4 4>&-)|egrep \* >file;exec 4>&- 
或者 
exec 4>&1;(ls you no;ls yes) 2>&1 1>
&4 4>&-|egrep \* >file;exec 4>&-
如果加两个条件：

（1）要求cmd1和cmd2并行运行；

（2）将cmd1的返回值赋给变量 ss。

则为：

exec 3>&1;exec 4>&1 
ss=$(((ls you no 2>&1 1>&3 3>&-;echo $? >&4)|egrep \* >file) 4>&1) 
exec 3>&-;exec 4>&-
说明：

exec 3>&1;4>&1 建立FD3，是用来将下面ls那条语句（子shell）中的FD1 恢复到正常FD1，即输出到monitor，你可以把FD3看作最初始的FD1的硬盘备份（即输出到monitor）；建立FD4，到时用作保存ls的返 回值（echo $?），你可以将FD4看作你考试时用于存放计算“echo $?”的草稿纸；

(ls you no 2>&1 1>&3 3>&-;echo $? >&4) 大家还记得前面说的子shell和管道吧。这条命令首先会继承FD0、FD1、FD2、FD3、FD4，它位于管道前，所以在运行命令前会先把子 shell自己的FD1和管道“|”相连。但是我们的条件是stderr通过管道送往egrep，stdout仍然输出到monitor。 于是通过2>&1，先把 子shell的FD1 的管道“送给”FD2，于是子shell中的stderr送往管道“|”；再通过 1>&3，把以前的“硬盘备份”恢复给子shell的FD1，于是子shell中的FD1变成送到monitor了。再通过3> &- ，将3关闭；接着运行echo $? ，本来其输出值应该送往管道的，通过 >&4 ，将 输出 送往 “草稿纸”FD4，留以备用。


((ls you no 2>&1 1>&3 3>&-;echo $? >&4)|egrep \* >file) 于是，stderr 通过管道送给 egrep ，stdout 送给monitor，但是，还有 FD4，它送到哪去了？ $(((ls you no 2>&1 1>&3 3>&-;echo $? >&4)|egrep \* >file) 4>&1)最后的 4>&1 ，就是把FD4 重定向到 FD1。但由于其输出在 $( )中，其值就赋给变量ss了。最后一行关闭 FD3、FD4。

6、 高阶例子

命令 cmd1, cmd2, cmd3, cmd4. 如何利用单向管道完成下列功能：

1. 所有命令并行执行。

2. cmd1 和 cmd2 不需要 stdin。

3. cmd1 和 cmd2 的 stdout 定向到 cmd3 的 stdin。

4. cmd1 和 cmd2 的 stderr 定向到 cmd4 的 stdin。

5. cmd3 的 stdout 定向到文件 a, stderr 定向到屏幕。

6. cmd4 的 stdout 定向到文件 b, stderr 定向到屏幕。

7. cmd1 的返回码赋给变量 s。

8. 不能利用临时文件。

解决方法：

exec 3>&1; exec 4>&1 
s=$(((((cmd1 1>&3 ; echo $? >&4 )| cmd2 ) 3>
&1 | cmd3 >a 2>&3 ) 2>&1 | cmd4 >b ) 4>&1) 
exec 3>&-; exec 4>&-
这 个我一步步解释（好复杂，自己感觉看明白了，过一会再看，大脑仍然有几分钟空白～～～，没想到我也能看明白。exec 3>&1; exec 4>&1 前面的例子都有说明了，就是建立FD3 ，给cmd1恢复其FD1用和给cmd3 恢复其FD2用，建立FD4，保存“echo $?”输出值的“草稿纸”。

第 一对括号：(cmd1 1>&3 ; echo $? >&4 ) 和其后（第一个）管道。在第一个括号（子shell）中，其FD1已经连到 管道中了，所以用 FD3 将 FD1恢复正常，不让他往管道跑；这里的cmd1没有stdin，接着将 cmd1 运行的返回码 保存到 FD4 中。


第 二对括号：((cmd1 1>&3 ; echo $? >&4 )| cmd2 ) 3>&1 和其后（第二个）管道。前面的 FD1 已经不送给 cmd2了，FD2 默认也不送过来，所以cmd2 也没有stdin ，所以在第二对括号里面：cmd1和cmd2 的stdout、stderr 为默认输出，一直遇到 “3>&1”为止。请注意：“3>&1”，先将第二对括号看出一个命令，他们遇到 第二个管道时，其FD1 连到 管道 “|”，由于“3>&1”的作用，子shell的FD1 送给FD3 使用，所以所有FD3 的输出都 “流往”cmd3，又由于继承关系（继承第一行的命令），FD3实际上就是cmd1和cmd2的stdout，于是“ cmd1 和 cmd2 的 stdout 定向到 cmd3 的 stdin”

第 三对括号：(((cmd1 1>&3 ; echo $? >&4 )| cmd2 ) 3>&1 | cmd3 >a 2>&3 ) 2>&1 和其后的第三个管道。cmd1 和 cmd2 的 stdout 已经定向到 cmd3 的 stdin，处理之后，cmd3 >a 意味着将其 stdout 送给 a 文件。而2>&3的意思是：恢复cmd3的错误输出为FD3，即送往 monitor。于是“cmd3 的 stdout 定向到文件 a, stderr 定向到屏幕”。如果没有“2>&3”，那么cmd3的错误输出就会干扰cmd1和cmd2的错误输出，所以它是必须的！请注意第三对括号后 的 “2>&1”| ，其子shell的FD1 本来连接着管道“|”，但子shell FD1 慷慨大方，送给了 FD2，于是FD2 连接着管道。还记得前面的 cmd1 和 cmd2 吗？他们的stderr一直没动了。于是在这里，通过管道送给了 第四个命令cmd4 了。即“cmd1 和 cmd2 的 stderr 定向到 cmd4 的 stdin”。后面就比较简单了。cmd4 >b 表示“cmd4 的 stdout 定向到文件 b, stderr 定向到屏幕（默认）”

第 四对括号：((((cmd1 1>&3 ; echo $? >&4 )| cmd2 ) 3>&1 | cmd3 >a 2>&3 ) 2>&1 | cmd4 >b ) 与其后的 4>&1。四对括号里面的 FD1、FD2都处理完了。但是还记得前面“echo $? >&4”那块“草稿纸”吗？“4>&1”的作用就是“将草稿纸上的内容送给monitor”，但是由于最外面还有 $() 将其“包着”。于是其值赋给变量“s”。

