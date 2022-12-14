### Linux基本信息

#表示超级管理员，$表示普通用户

关机命令：shutdown -h now（正常关机）、halt（关闭内存）、init 0

使用VMware备份os：

- 快照：又称还原点，保存系统的状态（包含了所有的内容），后期时候可以随时恢复，侧重于短期、频繁备份，做快照时候虚拟系统一般处于开启

- 克隆：复制，侧重于长期备份，克隆时候虚拟系统必须关闭

Linux一切皆文件

目录结构

- bin：里面都是二进制文件

- dev：存放外界设备，例如盘，光盘等。外接欸设备是不能被直接使用的，需要挂在（类似win下分配盘符）

- home：除了root用户意外其他用户的家目录，类似win下User/用户目录

- proc：process，表示进程，该目录中存储Linux运行时候的进程

- root：root用户的家目录

- sbin：super binary，存放一些可以被执行的二进制文件，但是必须得有super权限的用户才能运行

- tmp：系统运行时候产生的临时文件

- usr：存放用户自己安装的软件，类似win下program files

- var：存放程序/系统的日志文件目录

- mnt：外接设备需要挂载时候，就挂在mnt目录下

指令格式：#指令 [选项] [操作对象]

一个指令可以包含多个选项和多个操作对象

##### 常用命令

- echo：往终端输出

- 终端里直接选中就是复制，右键鼠标就是粘贴

- man 指令：查看指令手册

- ls：列出当前目录下所有文件

- ls 路径：列出指定路径下所有文件   ./ 当前目录下   ../上一级目录下

- ls 选项 路径：列出指定路径下所有文件，以指定的格式
  
  - -l 以列表形式现实，-a 显示所有文件（包括隐藏文件）
  
  - ls -l path
  
  - ls -la path
  
  - 详细信息第一列是权限，其中第一列-表示文件，d表示文件夹
  
  - .开头是隐藏文件
  
  - ls -lh path：以列表形式列出，并且以可读性较高方式显示（其实就是显示占用大小从字节改成合适的单位，比如k，M，G）
  
  - 目录（文件夹）大小固定4k，不包含内部文件的大小，要查看文件夹实际大小，用du -sh命令
  
  - 选项顺序无所谓

- pwd：print working directory

- cd：change directory，格式：cd path

- ~：当前用户的家目录

- mkdir：make directory  格式：mkdir path
  
  - mkdir -p path：可以一次创建多级目录
  
  - mkdir path1 path2 ...：一次创建多个文件夹

- touch：创建文件  格式：touch filePath  也可以一次创建多个文件，和mkdir类似

- cp：copy，复制文件/文件夹到指定位置  格式：cp 被复制文件(夹)路径  复制到的路径      复制时候还可以顺便改名

- 当使用cp命令复制文件夹时候，选项要加-r，表示递归复制

- mv：move，格式：move 需要移动的文件(夹) 目标路径，对于文件夹**不用**加-r

- 重命名也是用mv命令

- rm：remove，删除文件(夹)，rm 选项 path
  
  - 不带选项，会提示确认
  
  - -f：force，强制，不提示
  
  - -r：递归，删除非空目录要用
  
  - 可同时删除多个文件(夹)
  
  - 删除公共名字的文件(夹)：可用这正则表达式：*通配符，表示任意数量任意字符

- vim：文本编辑器，格式：vim filePath  打开一个文件（可以不存在）
  
  - 退出打开的文件：在没有按下其他命令时候，按下shift+英文冒号，输入q然后回车即可

- 输出重定向：一般命令输出直接输出在终端里，有时候需要将一些命令的执行结果保存到文件中，此时便是输出重定向
  
  - \> ：覆盖输出，覆盖掉原先文件内容
  
  - \>> ：追加输出，在文件结尾追加内容
  
  - 语法：正常执行的指令 \>/\>> 保存的文件路径（可以不存在）

- cat指令
  
  - 有直接打开一个文件的功能（直接读取输出到终端）  格式：cat filePath
  
  - 可以对文件进行合并  格式：cat filePath1 filePath2 ... \>/\>> 合并后的filePath   如果不输出重定向 就只是打开多个文件而已

##### 进阶命令

- df：查看磁盘空间
  
  - 格式：df -h ，-h表示以适合阅读方式
  
  - 表头里的Mounted on：挂载点（路径）

- free：查看内存使用情况，-m以兆(M)为单位

- head：-n表示查看一个文件的前n行，若不指定则默认为10行，格式 head -n filePath

- tail：后n行，用法如head
  
  - tail -F filePath：查看一个文件的动态变化，手动追加内容也看得到
  
  - tail -f filePath：查看一个文件的动态变化，手动追加内容也看不到，只能看到自动追加的，比如重定向

- less：查看文件，以较少内容进行输出，按下辅助功能键查看更多   格式：less filePath

- wc：统计文件内容信息（行数，单词数，字节数） wc -lwc filePath
  
  - -l：line
  
  - -w：words
  
  - -c：character

- date：表示操作时间日期（读取，设置）
  
  - date
  
  - date +%F：等价于date "+%Y-%m-%d"，注意大小写要严格，例如大写的M表示分钟
  
  - date "+%F %T"，引号让后面成为整体，等价于
    
    date "+%Y-%m-%d %H:%M:%S"
  
  - 获取之前或者之后的某个时间：date -d "-1day " "+%Y-%m-%d"
    
    - +：之后    -：之前
    
    - day，month，year

- cal：操作日历
  
  - cal：直接输出当前月份日历
  
  - -3：输出上个月这个月下个月
  
  - -y 2018：输出2018年的日历

- clear/ctrl+L：清除终端里的已经存在的信息（并不是真的清除，只是推到最上面了）

- 管道符：|，不能单独使用，必须配合指令，辅助用
  
  - 用于过滤
    
    - ls path | grep ab，列出包含ab的文件
    
    - 以管道作为分界线，前面命令输出就是后面的输入，然后过滤之后，然后输出
    
    - grep：主要用于过滤
  
  - 特殊用法：通过管道实现less命令等价效果（了解）：cat path | less
  
  - 扩展处理统计某个目录下文件(夹)总数
    
    ls / | wc -l

##### 高级命令

- hostname：操作主机名（读取）：
  
  - hostname：输出完整的主机名
  
  - hostname -f：输出当前主机名中的FQDN(Fully Qualified Domain Name)

- id：查看一个用户的一些基本信息（用户id，用户组id，附加组id...），该指令不指定用户则默认当前用户
  
  - 验证信息是否正确？
    
    验证用户信息：通过文件/etc/passwd
    
    验证用户组信息：通过文件/etc/group

- whoami：显示当前登录用户名，一般用于shell脚本

- ps -ef
  
  - ps：查看当前运行进程信息
  
  - -e：等价于-A，表示列出全部进程
  
  - -f：显示全部的列
  
  - UID：执行该进程的用户id
  
  - PID：进程id
  
  - PPID：该进程对应父进程id，如果一个进程的父进程找不到，则该进程称之为僵尸进程
  
  - C：cpu占用，形式是百分比
  
  - STIME：start time，进程的启动时间
  
  - TTY：终端设备，发起该进程的设备识别符号，如果是？，则该进程不是由终端设备发起
  
  - TIME：进程的执行时间
  
  - CMD：该进程的名称或者对应的路径
  
  - 在ps结果中过滤出想要查看的进程：ps -ef | grep 进程关键字：至少出现一个，也就是执行的命令本身

- top：查看进程占的资源，按q退出，表头里：
  
  - PR：pagerank，进程优先级
  
  - VIRT：虚拟内存：例如进程申请500MB，
  
  - RES：常驻内存：使用了300MB
  
  - SHR：共享内存：调用了其他进程，其他进程占的内存
  
  - 一个进程实际使用内存 = 常驻内存(RES) - 共享内存(SHR)
  
  - S：状态，S表示睡眠，R表示运行
  
  - 运行之后按
    
    - 注意大小写，大小写有影响
    
    - M：结果按内存占用降序排序
    
    - P：结果按CPU占用降序排列
    
    - 1：显示所有逻辑处理器

- du -sh path：查看文件夹真实大小
  
  - -s：summaries，只显示汇总的大小
  
  - -h：以较高可读性显示

- find：用于查找文件，参数很多
  
  - 用法：find 路径范围 选项 选项的值
  
  - -name：按照文件名搜索（支持模糊搜索）：find /home/zev -name a.txt
  
  - -type：按照文件类型搜索（-表示文件（搜索时候用f替换），d表示文件夹，非txt之类的类型）find /home/zev -type f
  
  - 有权限才能搜

- service：用于控制一些软件的服务启动/停止/重启，语法：service 服务名 start/stop/restart

- kill：杀死进程
  
  - 语法：kill 进程PID
  
  - 一般需要配合ps命令一起使用
  
  - killall 进程名称

- ifconfig：操作网卡相关指令
  
  - inet：ipv4地址

- reboot：重新启动系统
  
  - -w：模拟重启，但是不重启；（只写关机开机的日志信息，测试用）

- shutdown：关机
  
  - shutdown -h now  或者 shutdown -h 15:20 "文字提示" （定时关机，并显示文字提示；shutdown -c取消）
  
  - 除了shutdown，还有以下关机命令
    
    - init 0
    
    - halt
    
    - poweroff

- uptime：输出计算机持续在线时间，即开机以来到现在的运行时间

- uname：获取计算机操作系统的相关信息
  
  - -a：获取全部的系统信息

- netstat -tnlp：查看网络链接状态
  
  - -t：表示只列出tcp协议的链接
  
  - -n：表示将地址从字母组合转化成ip地址，将协议转化成端口号显示
  
  - -l：表示过滤出state列中值为LISTEN的链接
  
  - -p：表示显示发起链接的进程的PID和进程名称

- man：manual，手册；语法：man 指令；退出时候按q

- **删除终端中光标前/后的内容？ 前：ctrl+u；后：ctrl+k**

### vim使用

- vi是Unix/Linux的默认编辑器，vim是升级版，更适合coding

- vim主要的三种模式：命令模式、编辑模式、末行模式
  
  - 命令模式：该模式下不能对文件直接编辑，可以输入快捷键操作，如删除行、复制行、移动光标、黏贴等（打开文件之后默认进入的模式）
  
  - 编辑模式：可以对文件内容进行编辑
  
  - 末行模式：在末行输入命令对文件进行操作，如搜索、替换、保存、退出、撤销、高亮等

- 打开文件的方式
  
  - vim filePath
  
  - vim +数字 filePath：打开指定文件，并将光标移动到指定行
  
  - vim +/关键词 filePath：打开指定文件，并高亮显示关键词
  
  - vim filePath1 filePath2 ... ：打开多个文件

- 退出：shift+; （即输出一个冒号），然后按q回车

##### 模式间切换

- 英文冒号：从命令模式到末行模式
  
  - 退出末行模式：
    
    - 1下esc：延迟一下然后退出
    
    - 2下esc：直接退出
    
    - 退格删除直到删除冒号

##### 命令模式

- 打开文件默认的模式

- 光标移动到行首/尾：
  
  - 首：shift+6；实际是按下^
  
  - 尾：shift+4；实际是按下$

- 移动到首/尾行：
  
  - 首：gg 或者 Home
  
  - 尾：G 或者 End

- 翻屏：
  
  - 向上翻屏：ctrl+b 或者 PgUp （b：back）
  
  - 向下翻屏：ctrl+f 或者PgDn （f：front）

- 复制
  
  - 复制光标所在行：yy
  
  - 粘贴：想要粘贴的地方：p （paste）
  
  - 以光标所在行为准，向下复制指定行数：数字 yy
  
  - 可视化复制：ctrl+v，上下左右选中后按yy复制

- 剪切/删除：
  
  - 剪切/删除光标所在行，下面行上移：dd
  
  - 剪切/删除光标所在行为准，向下多行，下面行上移：数字 dd
  
  - 剪切/删除之后下一行不上移：D

- 撤销/回复：
  
  - 撤销：冒号+u（即shift+;，然后小写u   这个是末行模式）  实际上直接输入u也可以  （undo）
  
  - 恢复：ctrl+r （redo）

- 光标快速移动：
  
  - 快速移动光标到指定行：数字 G
  
  - 以当前光标为准，向上/下移动多行，向左/右移动多个字符：数字 ↑    数字 ↓   数字 ←   数字 →
  
  - 末行模式下快速移动到指定行：按下冒号 然后数字 回车

##### 末行模式

- 只能由命令模式进入

- 保存（write）：w

- 另存为：w  path

- 退出：q

- 保存并退出：wq

- 强制：!，不保存退出：q!

- 调用外部命令：在vim里面调用外部命令，暂时隐藏文件内容，显示外部命令输出，语法：! 命令

- 搜索/查找：/ 关键词，这也是进入末行模式的方式，不过一般仅限于搜索；在搜索结果中切换上/下一个结果：上：N，下：n

- 取消高亮：nohl

- 替换：
  
  - s/搜索的关键词/新的内容：替换光标所在行的第一处符合条件的内容
  
  - s/搜索的关键词/新的内容/g：替换光标所在行的全部符合条件的内容
  
  - %s/搜索的关键词/新的内容：替换文档中每行第一处符合条件的内容
  
  - %s/搜索的关键词/新的内容/g：替换文东中所有符合条件的内容
  
  - %表示整个文件，g表示全局（global）

- 显示行号：set nu；取消则：set nonu；只是临时显示的

- 打开多个文件，在末行模式下切换文件
  
  - files：显示打开的文件
    
    - %a：active，表示当前正在打开的文件
    
    - \#：表示上一个打开的文件
  
  - 切换文件的方式
    
    - 指定切换的文件名称：open fileName
    
    - 通过其他命令切换上/下一个文件
      
      - bn：下一个文件（back next）
      
      - bp：上一个文件（back previous）

##### 编辑模式

- 从命令模式进入，直接按字母键

- i：在光标所在字符前开始插入

- a：在光标所在字符后开始插入

- 1下esc退出

##### 实用功能

- 代码着色
  
  - 开启：末行模式下：syntax on
  
  - 关闭：末行模式下：syntax off

- vim计算器的使用：当在编辑文件的时候要计算一些公式，则要用计算器
  
  - 进入编辑模式
  
  - 按下ctrl+R，然后输入=，此时光标变到最后一行
  
  - 输入 计算内容，回车

##### 扩展内容

- vim配置
  
  - 在文件打开时候在末行模式里输入的配置（临时的）
  
  - 个人配置文件（在~/.vimrc目录下，不一定存在）
    
    - 新建好个人配置文件
    
    - 编辑输入：set nu
  
  - 全局配置文件（vim自带，/etc/vimrc）：编辑输入同个人配置文件
  
  - 配置文件以个人的优先

- 异常退出：未正常保存就关闭了  解决方法：删除交换文件就可以了

- 别名机制：相当于创建一些属于自己的自定义命令
  
  - 通过一个别名映射文件：~/.bashrc
  
  - 更改文件之后要生效要重新登录当前用户

- 退出方式
  
  - 之前退出编辑的文件用：q或者：wq
  
  - 另一个保存退出方法：x，将上面合二为一，改变了文件内容就是保存后退出，不然就是直接退出
  
  - 如果文件未被修改，但是使用wq退出，则文件的修改时间会被更新；但是用x退出不会更改修改时间
  
  - 不要使用X！！！（X用来对文件加密，保存后生效）取消加密的话再X一边，然后两次空密码回车则解取消加密

### Linux自有服务

不需要用户安装，系统自带的服务

##### 运行模式

也可以称之为运行级别

linux中存在一个进程：init（initialize，初始化），PID是1。该进程存在一个对应的配置文件：inittab（系统运行级别配置文件，位于/etc/inittab）<u>目前已经被淘汰了</u>

- 0：关机级别（不要将默认运行级别设置成这个值）：init 0执行的就是这个进程

- 1：单用户模式

- 2：多用户模式，不带NFS（Network File System）

- 3：多用户模式，完全体：init 3，需要root权限

- 4：未被使用，保留

- 5：X11，完整的图形界面模式

- 6：重启级别（不要将默认运行级别设置成这个值）

##### 用户与用户组管理（重点）

- 三个文件
  
  - /etc/passwd：存储用户关键信息
  
  - /etc/group：存储用户组关键信息
  
  - /etc/shadow：存储用户密码信息

- 添加用户
  
  - 格式：useradd 选项 用户名
  
  - 常用选项
    
    - -g：指定用户主组，值可以是用户组id或者组名
    
    - -G：指定用户附加组，值可以是用户组id或者组名
    
    - -u：uid，用户的id，系统默认从500之后顺序分配，比喜欢也可以通过该选项自定义
    
    - -c：添加注释，comment
  
  - 验证是否成功创建新用户：1.验证/etc/passwd下有没有新添加用户信息  2.验证是否有家目录
  
  - passwd文件格式：用户名:密码:UID:用户组ID:注释:家目录:解释器shell
  
  - 不添加选项，执行useradd之后会执行一系列操作：
    
    - 创建同名家目录
    
    - 创建同名用户组

- 修改用户
  
  - 格式：usermod 选项 用户名
  
  - 常用选项
    
    - -g
    
    - -G
    
    - -u
    
    - -c
    
    - -l：小写L，表示修改用户名
      
      usermod -l 新用户名 旧用户名：把-l 新用户名整体看成选项

- 设置密码
  
  Linux不允许没设置密码的用户登录到系统，所以刚创建好的用户都处于锁定状态
  
  - 格式：passwd 用户名
  - passwd不加参数为设置当前登录账户密码
  - 设置完就能登录了
  - su：switch user
    - 格式：su 用户名，**如果用户名不指定则切换到root用户**

- 删除用户
  
  - 格式：userdel 选项 用户名
  
  - -r：删除同时删除其家目录
  
  - 登录中的用户删除不了：kill 对应用户的全部进程

- 和用户操作的有关的命令（除了passwd），只有root有权限执行

- 用户组管理
  
  对同一个用户组的用户进行集中管理
  
  - 设计用户组的添加、删除和修改。实际上就是对/etc/group文件的更新。
  
  - 文件结构：用户组名:密码:用户组ID:组内用户
  
  - 用户组添加：groupadd 选项 用户组名
    
    - -g：类似用户添加的-u，选择自定义用户组ID，不指定的话默认从500递增
  
  - 用户组编辑：groupmod 选项 用户组名
    
    - -g：设置自定义用户组ID
    
    - -n：设置新的用户组名称
  
  - 用户组删除：groupdel 用户组名
    
    - 若删除的组是主组，得移除组内所有用户

##### 网络设置

- 配置文件路径：/etc/sysconfig/network-scripts/（在我的ubuntu里没有）

- 网卡配置文件：ifcfg-网卡名称

- 重启网卡命令：service network restart 或者 
  /etc/init.d  这个目录下放了很多服务的快捷方式
  /etc/init.d/network restart

- 快捷方式（软连接）：ln -s 目标路径 快捷方式路径
  
  - 在ls -l里文件类型是l，表示link

- 重启单个网卡：
  
  - 停止某个网卡：ifdown 网卡名
  
  - 开启某个网卡：ifup 网卡名

##### SSH(secure shell, 安全外壳协议)

- 远程链接协议、文件传输协议

- 协议使用的端口：默认22

- 配置文件：/etc/ssh/ssh_config

- 服务：service sshd start/stop/restart

- 远程终端
  
  - 工具：Xshell、secureCRT、Putty
  
  - 在windows终端里输入：ssh ip地址，然后输入用户名密码即可链接
  
  - exit：退出当前shell，可以用来退出当前ssh链接

- 传输文件：在Windows终端使用sftp命令
  
  - 连接远程服务器：（即远程服务器ip）
    
    - sftp [zev@192.168.183.129](mailto:zev@192.168.183.129)
    
    - sftp [zev@192.168.183.129](mailto:zev@192.168.183.129)：/home/zev
  
  - 上传
    
    - 上传文件：put local-file remote-path
    
    - 上传文件夹：put -r local-dir remote-path
    
    - 上传多个文件（支持通配符）：mput local-files remote-path（例如mput a*.txt remote-path）
    
    - 上传多个本地目录（支持通配符）：mput -r local-dirs remote-path
  
  - 下载：get，用法类似上传
  
  - 当用sftp命令登录上远程服务器后，相当于直接连入了远程服务器，直接可以执行命令
  
  - 若要执行本地命令，两种方法
    
    - 部分命令只需要在常规命令前加l（小写L）
    
    - 在命令前加 !
  
  - 退出：exit
  
  - linux向Windows传文件可以用scp命令，具体上网查

##### 设置主机名

- 临时设置：hostname 临时主机名，重启失效

- 永久设置：改配置文件：/etc/sysconfig/network（我的ubuntu是/etc/hostname）

- 在/etc/hosts文件中可以修改本机的DNS文件

- 如果不设置好FQDN：
  
  - 很多开源服务器软件可能无法启动，或报错
  
  - 方便记忆
  
  - 不设置，影响本地域名解析

##### 自启动管理：chkconfig

- 开机启动项的管理服务：chkconfig --list（ubuntu是systemctl list-unit-files）

- 可以自行添加，查看，删除
  
  - 查看：chkconfig --list
  
  - 删除：chkconfig --del 服务名
  
  - 添加：chkconfig --add 服务名
  
  - 注意，此处只是添加到管理列表里，具体是否开机启动还要设置
  
  - 设置某个服务在某个级别下开机启动/不启动
    
    - chkconfig --level 数字级别 服务名 on/off
    
    - 可以多个级别，连在一起写就可以了

##### ntp服务

- 用于对于计算机的时间同步管理操作

- 时间上游：时间服务器的域名或者ip

- 设置服务器时间
  
  - 手动同步：ntpdata ip
  
  - 自动同步服务：
    
    - 服务名：ntpd
    
    - service ntpd start 或者 /etc/init.d/ntpd start

##### 防火墙服务

- 防范一些网络攻击：选择性让请求通过

- 操作
  
  - centos5里这么操作，新的和ubuntu不是这么操作了
  
  - service iptables start/restart/stop/status
  
  - /etc/init.d/iptables start/restart/stop
  
  - 对于init.d，如果要进到目录里执行比如network，要用./network start，直接用network start系统会当成可执行命令，到系统路径里去找了
  
  - service iptables status 也可以写 iptables -L ，-L表示列出规则

- 简单设置：
  
  - iptables -A INPUT -p tcp --dport 80 -j ACCEPT：允许访问80端口
    
    - -A：add，添加
    
    - INPUT：进站请求
    
    - -p：protocol，协议
    
    - --dport：端口号
    
    - -j：行为，允许accept或禁止reject
  
  - 添加完成后要有个保存操作：/etc/init.d/iptables save
  
  - -A换成-I成功（大写i）

##### rpm管理

- 软件管理

- 查询：rpm -qa | grep 关键词
  
  - -q：query，查询
  
  - -a：all，全部

- 卸载：rpm -e 软件名
  
  - 没有依赖关系的包可以直接卸载
  
  - 有依赖的包：可以强制卸载，不考虑依赖关系：rpm -e 软件包名 --nodeps

- 软件安装：
  
  - 软件官网
  
  - 镜像文件中
    
    - 查看块状设备的信息：lsblk：list block devices
    
    - MountPoint：挂载点
      
      - 解挂：umount 挂载路径，相当于在Windows上点了弹出，但是还没拔出U盘
      
      - 挂载：mount 设备原始地址 要挂载的路径
        
        - 设备原始地址：都在/dev下
        
        - 挂载路径：一般都在mnt下
  
  - 命令：rpm -ivh 软件包完整名
    
    - -i：install
    
    - -v：进度条
    
    - -h：以#显示进度条

##### cron计划任务

- 为了在指定时间点执行任务，所以可以交给计划任务程序去操作。

- 格式：crontab 选项
  
  - -l：list，列出指定用户的计划任务列表
  
  - -e：edit，编辑指定用户的计划任务列表
  
  - -u：user，指定用户名，不指定的话默认当前用户
  
  - -r：remove，删除指定用户的计划任务列表

- 编辑：
  
  - 以行为单位，一行就是一个计划
  
  - 格式：分 时 日 月 周 需要执行的命令
  
  - *表示所有的时间，比如 0 0 * * *表示每周每月每日的0点0分
    
    - 分：0~59
    
    - 时：0~23
    
    - 日：1~31
    
    - 月：1~12
    
    - 周：0~6，0表示星期天
    
    - *：取值范围里的每个数字
    
    - -：区间
    
    - /：每多少个，例如每10分钟执行一次：*/10
    
    - ,：多个取值
  
  - 例子：隔一天上午8点到11点的第3和第15分钟执行一次重启：3,15 8-11 */2  *  *  reboot
  
  - root管理员可以配置文件来设置某些用户不可以创建计划任务（白名单）：/etc/cron.deny，往里面写用户名，一行一个就行了；同时还有个白名单/etc/cron.allow，优先级高于黑名单

### Linux权限

与用户和用户组操作是兄弟操作

- Linux系统一般将文件可存/取访问的身份分为三个类别：owner、group、others，且三种身份各有read、write、execute等权限

- 在Linux中分别由读、写、执行权限
  
  - 读：对于文件夹，影响用户是否可以列出目录结构；对于文件，影响用户是否可以查看用户内容
  
  - 写：对于文件夹，影响是否可以在文件夹下创建/删除/复制到/移动到；对于文件，影响用户是否可以编辑
  
  - 执行：对于文件来说的，特别是脚本文件

- 身份
  
  - owner：所有者，默认为文档的创建者
  
  - group：与文件所有者同组的用户
  
  - other：其他人，相对于所有者（组外人）
  
  - root：神

- 权限：ls -l path中列出来的第一列
  
  - 一共10个字母，r表示read，可读；w表示write，可写；x表示execute，可执行；-表示没有对应权限
  
  - 都是rwx三个参数的组合
  
  - 第1位表示文档类型：-为文件，d为文件夹，l表示软连接
  
  - 2-4位为文件所有者权限，5-7位为文件所属用户组权限，8-10位位其他人对这个文件的权限

- 设置：
  
  - 格式：chmod 选项 权限模式 文档
    
    - -R：递归设置权限（当文档类型是文件夹的时候）
    
    - 权限模式：该文档要设置的权限信息
  
  - 只能是root用户或者文档所有者才能设置权限
  
  - 字母形式设置权限
    
    - u：user（owner）
    
    - g：group
    
    - o：others
    
    - a：all，不写的话默认为a
    
    - r：读
    
    - w：写
    
    - x：执行
    
    - +：给某用户新增权限
    
    - -：删除权限
    
    - =：设置为某权限
    
    - 例子：给某文件设置权限
      
      - chmod u+x,g+rx,o+r abc.txt
      
      - chmod u=rwx,g=rx,o=r abc.txt
      
      - 当一个文件所有用户都可以执行的时候，该文件显示为绿色
  
  - 数字形式：以rwx形式，有权限位置为1，所以r-x为101，即十进制的5；其他的类似
    
    - 所以可以如下命令设置：chmod 756 abc.txt
  
  - 一个小特殊：不要设置-wx这种权限，只能写不能读，无法用vim打开写入，但是可以用输出重定向写入

- 注意事项：如果要删除一个文件，不是看文件有没有对应的权限，而是看文件所在的目录有没有写权限，有才可以删除

- 属主和属组设置
  
  - 属主：所属的用户（文件的主人）：对应ls -l命令第3行
  
  - 属组：所属的用户组：对应ls -l命令第4行
  
  - 这两项信息在文件创建的时候会使用创建者的信息（用户名，所属的主组），如果删除了某个用户，则对应的信息就要修改
  
  - chown：change owner，更改文档所属用户
    
    - 格式：chown [-R] username path
    
    - -R，对于文件夹
  
  - chgrp：更改文档所属用户组
    
    - 格式：chgrp [-R] groupname path
  
  - 可以通过chown来修改用户和用户组
    
    - 格式：chown [-R] username:groupname path

##### 扩展

- sudo（switch user do）：可以让root用户定义某些特殊命令哪些用户可以执行

- 默认没有除root以外用户的规则，要使用得先配置

- 配置文件路径：/etc/sudoers，就算以root用户打开也是只读，配置需要用visudo命令

- root：用户名，加个%在前面表示用户组；ALL表示允许登录的主机（地址白名单）；(ALL)：表示以谁的身份执行，空白默认root；最后一个ALL：当前用户可以执行的命令，用,分割

- 写sudo规则时候不建议直接写命令，而是写命令的完整路径。可使用which命令来查看。格式：which 命令

- 添加好规则后，普通用户执行的时候要加sudo在开头执行命令；之后要当前用户密码确认，然后接下来5mins内执行sudo命令不需要再用密码

- 特别注意：如果执行/usr/bin/passwd给某个普通用户，此时普通用户可以更改root用户密码，所以得加一条!usr/bin/passwd root；这样将passwd root设置为例外情况

- 普通用户如何查看自己的特殊权限：sudo -l （小写L）

### Linux网络基础

- ping：检测当前主机和目标主机之间的连通性，非100%准确，有服务器禁ping
  
  - 语法：ping 主机地址（ip，主机名，域名都可）
  
  - 跨平台，linux默认一直ping，Windows只会ping4个

- netstat t -tnlp：查看网络链接状态
  
  - -t：表示只列出tcp协议的链接
  
  - -n：表示将地址从字母组合转化成ip地址，将协议转化成端口号显示
  
  - -l：表示过滤出state列中值为LISTEN的链接
  
  - -p：表示显示发起链接的进程的PID和进程名称

- traceroute：查找当前主机与目标主机间所有的路由器（会给沿途各个路由器发送icmp数据包，路由器可能不给响应）

- arp：根据ip获取mac的协议
  
  - 常用命令：arp -a：查看本地缓存表
  
  - arp -d 地址：删除某个主机地址

- tcpdump：抓包，抓取数据表
  
  - tcptump 协议 port 端口号
  
  - tcpdump 协议 port 端口 host 地址
  
  - tcpdump -i 网卡

### shell基础

- 是一个用c语言编写的程序，是用户使用linux 的桥梁。既是一种命令语言，又是一种程序设计语言。
- 提供一个界面，用户通过这个界面访问操作系统的内核服务。
- 指定某用户不允许登录系统：usermod -s /sbin/nologin 用户，-s代表指定shell，此处为指定shell为nologin

##### 入门

- 开头写上：#!/bin/bash，告知系统这个脚本要使用的shell解释器

- 命名规范：文件名.sh

- 运行时候要写成：./test.sh，运行其他的二进制程序也一样。如果直接写test.sh，系统会去系统变量path里找test.sh命令。通常只有/bin，/sbin，/usr/bin，/usr/sbin等在path里。

- 流程
  
  - 创建.sh文件：touch或者vim
  
  - 编写代码
  
  - 执行：必须得有执行权限

- 语法
  
  - 输出命令：echo 
    
    - 输出字符串要带引号
    
    - 变量不存在，至少会有空行

- 变量
  
  - 定义：var_name="var_value"；**<u>注意=两边不能有空格</u>**
  
  - 使用时候在变量名前要用一个$
  
  - 双引号能识别变量，单引号会原样输出；双引号能实现转义，单引号不行
  
  - 定义一个变量输出时间：d=\`date +'%F %T'\`
    
    反引号：需要在脚本中执行指令并将结果赋值给变量的时候要用

- 只读变量：readonly 变量名
  
  - 不用在前面加$

- 接受用户输入
  
  - 语法：read -p 提示信息 变量名

- 删除变量（了解）：unset 变量名，不用加$

- 条件判断
  
  ```shell
  if [condition]
  then
      cmd1
      cmd2
      ...
  elif [condition2]
      ...
  else
      ...
  fi
  ```

- 写成单行的形式：`if[condition];then cmd;fi`

- 运算符表达式：\`expr \$a + \$b\`
  
  - bash不支持简单数学运算，所以要通过expr实现
  
  - **<u>表达式和运算符之间一定要空格</u>**
  
  - 条件表达式 <u>**中括号也要空格**</u>：[ \$a == \$b ]
  
  ```shell
  if [ $a == $b ]
  then
  echo 'a=b'
  else
  echo 'a!=b'
  fi
  ```

- 关系运算符
  
  - 只支持数字或者数字字符串，不支持字母字符串
  
  - -eq（equal）：检测两个数是否相等
  
  - -neq（not equal）：检测两个数是否不相等
  
  - -gt（great than）：检测左边数是否大于右边数
  
  - -lt（less than）：检测左边数是否小于右边数
  
  - -ge（great than or equal）：大于等于
  
  - -le（less than or equal）：小于等于
  
  - 例子：[ \$a -ge \$b ]

- 逻辑运算符
  
  - !：非运算
  
  - -o：或运算
  
  - -a：与运算
  
  - 例子：[ \$a -lt 20 -o \$b -gt 2 ]

- 字符串运算符
  
  - =：
  
  - !=：[ \$a != \$b ]
  
  - -z：检测字符串长度是否为0；[ -z $a ]
  
  - -n：检测字符串长度是否不为0
  
  - 检测字符串是否为空，不为空返回true；[ \$a ]

- 文件测试运算符（重点）
  
  - 检测文件的属性
  
  - -b file：检测文件是否为块设备文件
  
  - -c file：检测文件是否是字符设备文件
  
  - -d file：检测文件是否是目录
  
  - -f file：检测文件是否为普通文件
  
  - -r file：是否刻度
  
  - -w file：是否可写
  
  - -x file：是否可执行
  
  - -s file：是否为空（字节是否大于0）
  
  - -e file：检测文件（夹）是否存在
  
  - 例子：[ -e \$file ]
  
  - 注意：权限判断，不考虑用户  有一个符合即成立

- shell脚本附带选项
  
  - 即脚本接收参数，和命令一样
  
  - 接收用\$1，\$2，...。直接用就可以了。\$0表示执行的脚本自己，\$1表示第一个参数，\$2表示第二个参数，以此类推。
  
  - 然后使用别名，就可以将脚本转换成命令执行

### MySQL基础

##### linux下软件安装方式

- 源码包
  
  - 优点：一般是开源软件，能自己改源码，编译安装可自选安装内容
  
  - 缺点：安装步骤较多，易出错，编译时间长
  
  - 解包：
    
    - tar -zxvf xxx.tar.gz（大多数）
    
    - tar -jxvf xxx.tar.bz2
    
    - 选项和压缩包选择不能乱
      
      - -z或--gzip或--ungzip：通过gzip指令处理文件
      
      - -x或--extract或--get：从压缩文件中还原文件
      
      - -v：显示操作过程
      
      - -f或--file：指定一个文件
      
      - -j：支持bzip2解压文件
  
  - 进入解压后的文件夹
    
    - 配置程序（config/configure/bootstrap）：指定安装目录、需要的依赖的路径、选择可选依赖、配置文件路径、通用数据存储位置等。
      
      - 指定安装的路径：--prefix=路径
      
      - 需要依赖的路径：--with-PACKAGE 包名=包路径
      
      - 不需要依赖：--without-PACKAGE 包名
      
      - 运行配置文件的时候记得加./   即./configure --prefix=/usr/local/xxx
    
    - 编译程序（make/bootstrapd）
    
    - 安装程序（make install/bootstrapd install）
    
    - 编译和安装可以一起操作，也可以分开：make && make install

- 二进制包（rpm）
  
  - 优点：包管理系统简单，只需要几个命令就可以实现包的安装，升级，查询和卸载
  
  - 缺点：经过编译，看不到源码
  
  - rpm相关指令
    
    - rpm -qa | grep 关键词
    
    - rpm -e 关键词 [--nodeps]：卸载某个包
    
    - rpm -ivh 完整名称
    
    - rpm -Uvh 完整名称：升级某个包
    
    - rpm -qf 文件路径：查询指定文件属于哪个包

- yum等（apt）傻瓜式安装
  
  - 优点：简单快速
  
  - 缺点：完全丧失自定义
  
  - 指令
    
    - yum list：当前已经装了和可以装的所有包
    
    - yum search 包名：按关键字搜索包、
    
    - yum [-y] install：-y表示默认允许
    
    - yum [-y] update 包名：不加包名更新所有包
    
    - yum [-y] remove 包名：卸载某个包

##### MySQL安装

- 安装服务器端：yum install mysql-server

- 启动：service mysqld start

- 配置：mysql_secure_installation

- 查看mysql端口号：netsata -tnlp；默认端口号为3306

- 输入mysql的root用户密码，默认没有时候直接回车，则会跳到设置root密码

- 是否移除匿名用户？选yes

- 是否允许root远程登陆（默认不允许）老师说选yes或者no没区别，都不允许

- 是否移除测试数据库？

- 重新加载权限表？yes（更改用户之后一般都要重载）

##### 启动控制

- service mysqld start/stop/restart

- 进入mysql方式
  
  - mysql -u 用户名 -p 
  
  - 然后输入密码，如果在-p后面输入密码时会明文显示

##### 默认目录/文件位置

数据库存储目录：/var/lib/mysql

配置文件：/etc/my.cnf

##### MySQL的基本操作

-  数据库里面有多个数据表，数据表由记录（行）和字段（列）组成

- 在MySQL中执行
  
  ```sql
  SHOW DATABASES;
  CREATE DATABASE 库名;
  DROP DATABASE 库名;
  USE DATABASE 库名;  
  ```

##### 备份

- 全量备份（数据+结构）注意是命令行执行：mysqldump -root -p123456 -A > 备份路径
  
  - -A表示全部数据库

- 指定库备份：mysqldump -root -p123456 数据库名 > 备份路径

- 指定多个库：mysqldump -root -p123456 --databased 库名1 库名2 ... > 备份路径

- 被分成xxx.sql会比较好

- 案例：每分钟备份一次test数据库：test.sh
  
  ```shell
  #!/bin/bash
  filename="test"`date +'%Y%m%d%H%M%S'`".sql"
  mysqldump -uroot -p123456 test > /root/$filenmae
  ```
  
  计划任务：crontab -e：* * * * * /root/test.sh

##### 还原

- 主要由两种方法：
  
  - mysql命令行source方法
  
  - 系统命令行方法

- 还原全部数据库
  
  - mysql命令行：source 备份文件路径（主要用这个）
  
  - 系统命令行：mysql -uroot -p123456 < 备份文件路径（输入重定向）

- 还原单个数据库
  
  - use 库名
  
  - source 备份文件路径

- 设置链接字符集，在MySQL命令行里：set names utf8

##### 远程链接

- 在sql中执行：select host,user from user查询可以登录的主机和用户
