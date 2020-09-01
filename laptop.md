# 笔记本电脑

Author: Daniel (danielw10001@gmail.com)

## 1. 重要参数简介

- 部件的安装方式: 板载: 一般为BGA焊接在PCB上的, 不能更换; 插槽: 可以自行更换
- CPU: 大脑. 买AMD 4000系列. 4核心8线程以上就够日常使用. 如果有渲染, 仿真需要, 可考虑8核心16线程标压处理器.
    - 性能排行:
        - 移动:
            - http://rank.kkj.cn/mcpu.shtml
        - 桌面:
            - https://www.mydrivers.com/zhuanti/tianti/cpu/
            - http://rank.kkj.cn/dcpu.shtml
    - 核心数: 可同时执行的任务数量
    - 超线程: 一个核心通过交替执行两个不同任务模拟两个核心的增强执行效率的特性
    - 时钟频率: CPU按周期工作, 一秒钟的周期数称为其时钟频率
        - 基础频率与加速频率: 工作少时, CPU运行在基础频率以降低功耗; 工作多时, CPU运行在比基础频率更高的加速频率来应对工作负载, 此时功耗成倍增加.
    - IPC (Instruction Per Circle): 一个时钟周期内一个核心能处理的指令数
    - 单线程性能: 顺序运算的能力, 由时钟频率和IPC决定
    - 多线程性能: 并行运算的能力, 由核心数, 线程数, 时钟频率和IPC决定
    - 制程: 晶体管大小, 由加工精度和工艺决定. 制程越小, 晶体管密度越高, 单位功耗和发热下运算能力越强
        - 目前, Intel使用 (已使用了6年的) 14nm工艺, 部分使用10nm工艺; AMD从2019年开始使用7nm工艺.
    - 功耗与TDP (Thermal Design Power): CPU消耗能量进行运算, 所消耗的一切能量都转化为内能, 因此必须进行散热, 否则温度过高将损坏CPU. 制程越小, 核心数越少, 时钟频率越低, 功耗越低, 所需散热能力越低. 散热能力与功耗相同时可达到热平衡, 能够长时间运行而不损坏CPU. 散热能力需要由TDP描述.
        - Intel的TDP参数是在基频下测出, 现实极限TDP往往是参数的3倍左右, 一般无法正常散热, 需要降低时钟频率和计算能力. AMD的TDP参数是在加速频率下测出, 可以代表现实TDP, 可以正常散热, 可以保持高计算性能.
    - 架构: 晶体管的排布方式. 决定所支持的指令集, 影响安全性, 极限频率, IPC.
        - 目前, Intel使用的架构已使用了6年, 因为老旧而有很多安全漏洞; AMD使用2019年启用的Zen2架构, IPC较Intel高17%左右.
    - 低压, 标压, 台式: 在不同平台上供电与散热的限制不同, 有三类CPU
        - 低压, 型号U (Under Voltage): 电压低, 功耗低, 发热低, 运算能力低, 但电能使用效率高. 性能弱, 但续航长.
        - 标压, 型号H (High Voltage): 电压高, 功耗高, 发热高, 运算能力强, 但电能使用效率较低. Intel标压CPU耗电极高, 续航极短. AMD标压CPU耗电正常, 续航正常.
        - 台式: 满血电压, 功耗, 发热, 运算能力, 性能几乎不受限制
    - 插槽: Intel为LGA, AMD为AM4和TR4
- CPU散热: 满负载运行95摄氏度以下, 键盘区域温度40摄氏度以下, 风扇噪音55分贝以下, CPU频率3.5Hz以上
- 内存: 高速存储器. 8GB够日常使用. 渲染和仿真需要16GB以上, 大型项目需要32GB以上. 推荐三星, DDR4, 3200MHz以上.
    - 频率: 一秒钟可存取的次数, 目前主流频率为3200MHz
    - 通道数: 插槽/接口数, 可独立通信的信道条数
        - AMD CPU更依赖高通道数的内存, 需要至少2内存通道来发挥性能
    - 读写速率: 一秒钟可读写的数据大小, 由频率与通道数共同决定
    - 插槽: 台式机为DIMM (Dual In-line Memory Module), 笔记本为SO-DIMM (Small Outline Dual In-line Memory Module)
    - 协议: DDR SDRAM (Double Data Rate Synchronous Dynamic Random Access Memory), 目前发展到DDR4
    - 颗粒/芯片: 内存通常由多个存储芯片组成, 单个存储芯片称为颗粒. 推荐使用三星颗粒.
- 硬盘: 低速高容量存储器. SATA SSD 512G够日常使用, 推荐三星EVO系列, 西部数据WD_BLACK系列, 铠侠EXCERIA系列, 避免660P系列. 若数据较多可选1TB. 避免内置机械硬盘. 避免SMR叠瓦式机械硬盘.
    - HDD (Hard Disk Drive) 机械硬盘: 数据以磁性方向的形式存储在磁片上, 由搭载电磁铁的机械臂定位, 读取和写入. 由于有机械部件, 容易损坏 (不能震动), 访问速度慢, 但是便宜, 容量大, 可以用作仓库, 但不适合作为日常用途.
    - SSD (Solid State Drive) 固态硬盘: 数据以电路通断的形式存储在集成电路上, 由电路逻辑定位和读取, 由电流熔断或接通电路写入. 没有机械部件, 不容易损坏, 访问速度高, 但是贵, 容量低, 适合作为日常用途. 需要注意存储单元使用寿命较低, 避免大量数据反复写入.
        - 颗粒/芯片: SSD通常由多个存储芯片组成, 单个存储芯片称为颗粒
    - 随机读写速度: 读写位置随机的数据的速度. 是影响系统和程序运行速度的速度. SSD > HDD.
    - 顺序读写速度: 读写位置连续的数据的速度. 是影响大文件拷贝和读取速度的速度. SSD > HDD.
    - 插槽形式: SATA速率已经足够, 感觉不明显.
        - 3.5英寸SATA: 占据空间大, 速率低
        - 2.5英寸SATA: 占据空间大, 速率低
        - M.2 SATA: 占据空间小, 速率低
        - M.2 PCIe NVMe: 占据空间小, 速率高
            - 宽度 (mm) 长度 (mm): 2230, 2242, 2260, 2280, 22110
            - 接口:
                - B: PCIe*2, SATA, USB2.0-3.0, Audio
                - M: PCIe*4, SATA
    - RAID (Redundant Array of Independent Disks): 多磁盘同步协作读写来模拟成一个磁盘的技术, 对可靠性与读写速率有巨大影响
        - 分为Raid 0-6共7中, 其中Raid 0在消费级产品中常见, 其余主要用于企业级产品
        - Raid 0降低可靠性, 极大提升读写速率; 其他类型Raid提升可靠性, 对读写速率影响不一
        - 根据控制模块的实现, 分为软Raid与硬Raid
            - 软Raid: 软件控制的Raid, 由系统运行的控制程序控制, 占用系统运算资源, 需要操作系统支持
            - 硬Raid: 硬件控制的Raid, 由独立的控制芯片及硬件控制, 不占用系统运算资源, 仅需要硬件与协议支持
- 屏幕: 避免OLED!!! 避免低频率PWM调光!!! 避免45%NTSC色域屏幕!!! 选取LCD, 100% sRGB色域, DC调光, 60Hz以上, delta E < 5的屏幕, 最大亮度400nit以上. 15英寸选取1440P (2K) 以上分辨率, 17英寸选取4K以上分辨率.
    - OLED, LCD, CRT: 屏幕原理
        - OLED (Organic Light-Emitting Diode): 单个自发光单元. 每个像素可自由关闭, 从而可以显示很深的黑色, 颜色很漂亮.
            - 但是发光单元寿命极低, 长时间显示相同图像 (如开始菜单栏), 由于不同发光单元损耗不同, 该图像将印在屏幕上无法去除, 只能更换 (可能要花钱).
            - 不同电压下颜色改变严重, 因此不能使用电压控制亮度, 只能使用通电时间占比来控制亮度, 低频率下伤害视觉系统.
        - LCD (Liquid Crystal Display): 背光发出混合光, 发光单元通过不同电压改变液晶晶体结构从而改变对不同频率光线的通透率, 从而显示颜色和图像.
            - 只能通过背光调整整体亮度, 不能调整单个像素的亮度, 黑色看起来是深灰色
            - 寿命长, 几乎无损耗
            - 不同电压下背光频率分布无改变, 可使用电压控制亮度而不影响颜色.
        - CRT (Cathode-Ray Tube): 由阴极射线射击到荧光涂料上发光来显示颜色与图像.
            - 微量辐射
            - 阴极射线管体积巨大, 显示器屁股特别大
            - 刷新率高, 响应延迟小, 适合电子竞技
            - 已被淘汰
    - 色域: 能够显示颜色的数量和范围, 有NTSC, sRGB, AdobeRGB等评估标准
        - 低色域颜色太难看...45%NTSC辣眼睛!
    - 色准: 显示颜色的准确度, 用delta E评估
        - delta E > 5 颜色不准, 辣眼睛!
    - 亮度: 光线强度, 单位nit
    - 调光方式: 改变亮度的方式
        - DC调光: 通过电压改变亮度, 无频闪, 不伤眼
        - PWM调光: 通过电路通电时间比率改变亮度, 有频闪, 低频时伤眼
    - 分辨率: 独立显示单元个数
    - PPI (Pixel Per Inch): 独立显示单元密度
    - 刷新频率: 每秒钟能够显示的图像个数, 电子竞技需要
    - 响应速度: 输入与图像改变的时间延迟, 电子竞技需要
    - 触摸屏: 软件支持不完善, 鸡肋
- 键盘: 标准美式布局, 无7字回车, 大回车, 短Shift键, 方向键占据Shift键位置的情况, 否则打字很难受! 薄膜键盘即可, 键程1mm以上, 避免独立小键盘 (打字偏左, 不在屏幕正中间, 很难受). 尽可能有独立ESC键与Fn键.
    - 原理: 触电短路电极
        - 机械键盘: 机械结构分离触电, 按下后触点短路两电极
        - 薄膜键盘: 弹性薄膜分离触电, 按下后触点短路两电极
- 电池: 70Wh以上, 99Wh最佳
- 接口: USB3.0+TypeA至少2个, 网线接口至少1个, HDMI/DP至少一个, 雷电3有最好没有也可以
- 触摸板: Windows Precision Driver! 否则很难用
- GPU: Nividia RTX 2060+, 用于仿真, 渲染与CUDA并行计算, MaxQ或标压均可
    - 性能排行:
        - 移动:
            - http://www.mydrivers.com/zhuanti/tianti/gpum/index.html
        - 桌面:
            - http://rank.kkj.cn/dgpu.shtml
            - http://www.mydrivers.com/zhuanti/tianti/gpu/index.html
    - AMD Radeon 没有CUDA, 不能进行CUDA编程与并行计算; 仿真软件支持弱, 避免购买
- 做工: 铝镁合金一体式CNC加工最好, 碳纤维也可以, 塑料的容易坏
- 操作系统: 主要取决于自己常使用的软件哪个操作系统有, 查一查支不支持Windows或MacOS就好
- 形态: 如无特殊需求, 购买普通形态. 可考虑Surface Pro或Surface Book形态, 但性价比低, 无特殊需求不考虑.

## 2. 参考资料

参数解析与科普:

https://space.bilibili.com/14871346/

https://zhuanlan.zhihu.com/p/64273719

https://zhuanlan.zhihu.com/p/24671709

https://zhuanlan.zhihu.com/p/27888975

https://zhuanlan.zhihu.com/p/102224874

https://zhuanlan.zhihu.com/p/84580494

https://www.zhihu.com/question/19811077

https://www.zhihu.com/question/31580417

产品一览:

https://mp.weixin.qq.com/s?__biz=MzIxMTAyNjk0OA==&mid=2654606744&idx=1&sn=58c048268c207ae44b08539b995a7bd6&chksm=8c9520f3bbe2a9e5ede0867c35df9888f8e2a8878cbfceafdee73972af2718fdfc6c3204e5a6

产品测评:

https://space.bilibili.com/12434430/

https://space.bilibili.com/489180728/

https://www.notebookcheck.net/ (参数齐全, 非常值得参考)

https://www.youtube.com/channel/UCXuqSBlHAE6Xw-yeJA0Tunw

https://www.youtube.com/channel/UCVYamHliCI9rw1tHR1xbkfw

