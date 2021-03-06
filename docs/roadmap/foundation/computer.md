# 计算机理论引导

?> 思考: 构成计算机需要哪些要素

* `状态`
* `输入`
    * 时钟作为一种标准输入 (时间驱动)
    * 另一种最好可以读取人类的想法
* `状态装换函数F (将输入和当前状态转换为下一个状态)`
* `输出`



?> 思考: 如何描述状态

* `数字就可以 - 一切皆可以用数字表示`
    * ASCII 编码
    * 动植物分类
    * 商品
    * 中文
    * Unicode 编码
* `一个数字不够 (需要数组)`



?> 思考: 状态转换函数是什么？

* 一个表格就可以，将接收到的输入 I 变成新的状态 S

|      | 0    | 1    |
| ---- | ---- | ---- |
| 0    | 1    | 1    |
| 1    | 0    | 0    |



?> 思考: 鼠标在 1024*768 的屏幕上有 786432 种位置；因此对应了 786432 种状态。如果用表格来描述需要很大的一个空间，如何优化？

* 方案：用程序描述状态转换表
    * 每次鼠标位置更新，都调用一段程序去计算需要显示的位置
    * 程序需要预先在机器中安装好
    * 需要有执行程序的工具（一个计算器）



## 输入

* `时间是最重要的输入`
    * 晶振 (**不是输入设备**)
* `可以通过设备读取外界信息`
    * 鼠标、键盘
    * 头盔
    * 脑电波



## 输出

* `从计算机中读取状态`
    * 找根线连接芯片的引脚
    * 打印机
    * 显示器



## 阿兰·图灵

* 英国 (1912 - 1954) 数学家、逻辑学家、密码分析家和理论生物学家，被誉为计算机科学和人工智能之父。
    * 图灵机
    * 图灵测试
    * 图灵完备
    * 可判定性



## 冯·诺依曼

* 匈牙利籍美国籍(1903 - 1957) 数学家，现代计算机理论和博弈论的奠基者。
    * 101 页报告 (EDVAC)
    * 算子理论、测度论、量子力学相关 

![image-20200812203528596](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghovwlnzb0j312o0pstd4.jpg)

## CPU + 内存

* 短期记忆 (很少事情)   ----  `寄存器(register)`
* 推力计算  ----  `算数逻辑单元(ALU)`
* 长期记忆 (很多事情)  ----  `随机存储器(RAM)`
* 其他 : 缓存 (L1, L2, L3)



##  指令

* LOAD A, 1000

![image-20200812205112081](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghowcwab98j30jt0a1jta.jpg)

| 计算 (ALU -  寄存器) | RAM - 寄存器 | 控制程序流程 |
| -------------------- | ------------ | ------------ |
| ADD                  | LOAD         | BRANCH       |
| SUB                  | STORE        | BREQ         |
| MUL                  | MOV          | BRNE         |
| AND                  |              | BRIO         |
| OR                   |              |              |
| SIN                  |              |              |
| ...                  |              |              |



## 指令入门 - Opcode、寻址模式和浮点数

* 计算机通过指令指挥计算机工作
* CPU 被时钟驱动，不断的读取PC指针指向的指令，并增加PC指针从内存中读取指令并执行。(如此周而复始)
* 不同的 CPU 架构使用不同指令。目前使用最广泛的是 `RISC (Reduced Instruction Set Computer, 精简指令集)`
* ![image-20200812211930248](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghox6cm43fj30ln0rgn3x.jpg)

* MIPS-32 指令示例
* ![image-20200812212052981](https://tva1.sinaimg.cn/large/007S8ZIlgy1ghox7s6i7mj30zk0awgqo.jpg)



## 助记符

* 32 位RISC指令集中，opcode是6位数，这太过于抽象，不好记忆，因此我们通常采用助记符来记忆他们。例如：
    * 0b000000 (add)
    * 0b000008 (addi)
    * 0b100000 (lb) 将字节从内存中读进来
    * 0b100011 (lw) 把一个word 从内存中读进来



## 寻址模式 (Addressing Model)

* 指令集的一部分，决定指令有几个操作符，地址如何计算。
* 