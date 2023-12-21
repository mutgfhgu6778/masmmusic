# 留学生日常作业/课程设计/代码辅导/CS/DS/商科，仅为漂洋过海的你❥
标签：Computer Science、Data Science、Business Administration，留学生编程作业代写&&辅导

## 个人介绍:
本人是一名资深码农，酷爱投资。

为您提供 CS , DS , 商科 编程作业代写

<img src="design2023866.jpg"  width="200" />

# 一、实验目的
掌握输入/输出指令的使用方法，并且完成一个具有复杂程序结构的输入/输出汇编程序。
# 二、预备知识
1、乐曲简谱中的每个音符及其节拍，在微机中对应了扬声器的发声频率和持续时间。其中简谱
音符与扬声器的发声频率的对应关系见下表：

|音符 |频率（Hz）| 音符 |频率（Hz）| 音符 |频率（Hz）|
|------|------|------|------|------|------|
|低 1| 130.81 |中 1 |261.63 |高 1 |523.52|
|低 2 |146.83| 中 2 |293.66|高 2 |587.33|
|低 3 |164.81|中 3 |329.63| 高 3 |659.26|
|低 4 |174.61| 中 4 |349.23 |高 4 |698.46|
|低 5 |196.00 |中 5 |392.00 |高 5 |783.99|
|低 6 |220.00 |中 6 |440.00 |高 6 |880.00|
|低 7 |246.94 |中 7 |493.88 |高 7 |987.77|

2、如何使 PC 机的扬声器发出指定频率的声音？下面简单介绍一下 PC 机的发声原理：
IBM-PC 系列机的主机箱装有一个小扬声器，系统板上的定时器 8253（或 8254）利用工作方式
3 产生一定频率信号，通过可编程的并行外围接口芯片 8255（或 8255A）控制其发音。扬声器的控
制驱动电路如下图所示。
可编程的并行接口芯片 8255 有三个 8 位的并行端口：A 口、B 口和 C 口。在 IBM 系列微机中，
BIOS 在开机自检后已将 8255 初始化为 A 口和 C 口用于输入，B 口用于输出。B 口的 I/O 端口地址
为 61H。
由图可见，8255 的 B 口的低两位用来控制扬声器驱动，当 61H 端口的 D0 位为“1”时，控制
8254 定时器产生驱动扬声器发声的音频信号，该位为“0”则不发信号。8254 有三个定时器，分为
0 号、1 号和 2 号定时器，驱动扬声器的是 2 号定时器，该定时器工作在方式 3，是一个频率发生器，
它负责向扬声器发送指定频率的脉冲信号。
输出端口 61H 的 D1 位为“1”或为“0”时，将使控制驱动器的与门电路接通或关闭，使 8254
所发出的音频信号能到达驱动器或被阻断。这样通过控制 D1 位的变化，可使扬声器接通和断开，控
制扬声器是否能发出声音。此外，通过控制 D1 位的通断时间，就能发出不同的音长。
故当 8255 输出端口 61H 的 D1 位为“1”时，在 61H 的 D0 位为“1”，8254 发出指定频率
的声音信号的前提下，声音信号通过与门到达驱动器驱动扬声器发声。即是，如要 8255 控制 8254
的 2 号定时器驱动扬声器发声，则需要的汇编命令如下：
 OR AL， 00000011B
 OUT 61H，AL
关闭扬声器的汇编命令如下：
 AND AL， 11111100B
 OUT 61H，AL
同时，定时器 8254 的 2 号定时器使用 1.19MHz 的基准频率，故若要 8254 驱动扬声器发出指
定频率的声音，则需要向 2 号定时器的计数常数寄存器（即 I/O 端口 42H）存放基准频率除以指定
频率的商（即 122870H/指定频率），该商需分两次送往 I/O 端口 42H，先送商的低字节，再送商
的高字节。同时，在使用定时器 8254 的 2 号定时器之前，需要初始化，即往 8254 的 2 号定时器
的控制寄存器（即 I/O 端口 43H）写控制字 0B6H：
 MOV AL， 0B6H
 OUT 43H，AL
 3、键盘扫描码
（1）使用 IN AL, 60H 指令将会得到按键的扫描码，并存储在 AL 中。
（2）键盘扫描码包含通码（mark）和断码（break）两种。
（3）当按下某个按键时，产生 mark 码，并产生一次 IRQ1 键盘中断；放开按键时，产生 break
码，并产生一次 IRQ1 键盘中断。因此一个按键从按下到放开，实际上共产生了两次键盘中断。
（4）break 码和 mark 码的关系：break 码是由 mark 码最高位置 1 得来，即：break = mark 
+ 0x80。
（5）使用键盘扫描码可以检测某个按键是否被按下或者弹起。
（6）实验所需的按键扫描码如下：

|按键（顺序为键盘布局）| mark |break|
|------|------|------|
|q~u |10h~16h |90h~96h|
|a~j |1eh~24h |9eh~a4h|
|z~m |2ch~32h |ach~b2h|
|Esc |01h |81h|

# 4、调试须知
（1）可在 EMU8086 里面进行程序的编辑和调试，但是最终需要在 DOSBox 里面使用
MASM+LINK 进行编译和链接。注意：MASM 对语法的要求会更加严格。
（2）极个别的 Windows 系统的个人计算机如果在调试过程中，出现代码编写正确但听不到声
音的情况，这是模拟 DOS 的缘故，需要到实地址的 DOS 平台上才能听到声音。建议使用 Ubuntu
（虚拟机亦可）+ DOSBox + MASM 的方式进行程序调试，此种方法不存在以上问题。附 DOSBox 
for Ubuntu 官方下载地址：https://packages.ubuntu.com/bionic/dosbox
# 三、实验内容
试设计一个程序，能够使用键盘中字母键模拟钢琴按键发音。其中，按照字母在键盘中的排列方
式，字母键 z/x/c/v/b/n/m 分别发出低 1—低 7 共 7 个低音音符，字母键 a/s/d/f/g/h/j 分别发
出中 1—中 7 共 7 个中音音符，字母键 q/w/e/r/t/y/u 分别发出高 1—高 7 共 7 个高音音符。按 ESC
键退出程序。当按键按下时持续发音，当按键弹起时停止发音。

## 作业代写价格:
|类型|美元|人民币|
|-----:|-----:|-----:|
|Assignment|$60-$120|¥400-¥800|

### 为了方便快速定价和确定是否代做，麻烦提供私聊的时候提供以下信息：
如果是作业，提供本次作业要求文档；如果是考试，提供考试范围和考试时间
一两句话概括一下作业任务或者考试任务，比如”可以帮忙实现一个决策树算法吗？”、”网络安全选择题考试，范围是内网横向移动知识点”
### 作业定价有两种方式：
    - 根据定价标准进行
    - 通过微信我们一起协商
## 联系方式

微信（WeChat）：design2023866

<img src="design2023866.jpg"  width="200" />

