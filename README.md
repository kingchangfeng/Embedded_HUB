# 项目简介
本项目是基于沁恒CH339W和DAP-Link的多功能USB拓展坞模块，包含6个USB2.0、读卡器、百兆网口、JATG、DAP-Link、usb转串口、usb转i2c、usb转spi和一键复位功能。
做这个拓展坞主要是市面上的要么不全要么太贵。。。。索性就根据自己需求做了一个，适合日常的嵌入式或硬件开发。

# 项目功能
- USB2.0
芯片最多能拓展出7个，我这里引出6个足够日常使用了，另外一个用来给DAP-Link使用，支持USB2.0 高速480Mbps、全速12Mbps和低速 1.5Mbps。
- DAP-Link
DAP-Link参考的开源的，主控使用stm32f103c8t6引出了DIO、CLK和RST引脚能够满足下载调试。使用CH339W的usb7端口用来与上位机通信。
- 读卡器
满足sd2.0规范（SDHC），最高支持32G的sd卡。
- 网口
支持10M/100M自适应，实测能跑满百兆。
- 一键复位
<img width="639" height="466" alt="468532a18e0e4405bb2115dbd8dbfced" src="https://github.com/user-attachments/assets/2a058452-252e-4439-9223-50a08ba741e4" />
在调试设备的时候难免会来回的去插拔usb口，或者下载烧录的时候也会插拔一下才能下载进去（来回插拔真的特别烦，尤其是看着乱成鸡窝的桌面），所以就加了这个复位按键，就是将LDO的使能脚拉低一下让整个板子断电，实测好用！
- USB转串口
自带的usb转串口可以用来查看日志省去了一个模块，波特率支持2400bps～6Mbps日常使用没有问题。
- USB转I2C、SPI和JATG
自带的转I2C和SPI需要自行编写API。
<img width="323" height="225" alt="微信图片_20260609213741_7_27" src="https://github.com/user-attachments/assets/ca1cdacb-a39b-4a7b-a550-d5529e199589" />

# 原理解析（硬件说明）
整体的接口电路都采用了标准的EMC设计，供大家学习参考，不想焊接可以完全不焊。
另外板子上是有两个TypeC的接口，一个是用来连接电脑，另一个是用来连接5V电源的，避免设备接入过多导致供电不足，常规USB2.0一般只有500mA或者800mA电流的输出，设备接的少的话只接一个电脑端的就行。
使用电源切换进行电源的区分，PCB上也做了丝印进行防呆，PC接电脑，DC接5V电源。5V电源的话直接用手机充电器就行，我用的小米的5V的话最高能有3A的输出。
<img width="827" height="742" alt="7ac48f8b5c5b478689be12dcdab1a3f2" src="https://github.com/user-attachments/assets/a24167cd-6633-4e3a-a306-fe9e1869737c" />
信号走线也都采用了阻抗设计，4层板使用3313的层叠结构，网口100欧差分，usb90欧差分，sdio只做了等长。下面是使用SI9000计算的阻抗值，表层走线参考相邻层。
<img width="945" height="325" alt="25caa9b5ec1e471f966a4bc82669e013" src="https://github.com/user-attachments/assets/658d2bf6-55ab-49d1-a78c-70109beb0247" />
<img width="944" height="334" alt="533d61d29cb84a17a04a1139d2efb042" src="https://github.com/user-attachments/assets/b3256c87-9770-44b3-9d13-ed618abcf3e7" />
然后就是DAP-Link的固件烧录，板子上预留了测试点可以使用1.27间距的探针进行下载烧录，下面是DAP-Link的开源地址可以自行编译，我自己编译的也放在了附件中。
https://github.com/ARMmbed/DAPLink
<img width="1706" height="1279" alt="微信图片_20260609213800_8_27" src="https://github.com/user-attachments/assets/b6bcd87d-910a-4abf-80f7-6a5a79849bdf" />
<img width="436" height="107" alt="微信图片_20260609212117_6_27" src="https://github.com/user-attachments/assets/5ee6e4e1-1689-4fc2-b216-a652b3acf150" />

# 注意事项
板子一共做了三个版本，1.0是四层板但是长度超过10cm不能白嫖了，就做了1.1改为了正方形，1.2的话是改为了双面板，取消了阻抗的参考层，那么阻抗值肯定也到不到我们计算的要求。目前我是用的这一版，功能都还正常。
封装大多使用0402，使用焊台的话还算比较好焊接，焊接不熟练的话可以去掉esd（esd焊起来比较麻烦）。
