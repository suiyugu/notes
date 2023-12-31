#### RFID Security and Privacy: A Research Survey

自动识别物体和人

RFID（射频识别）技术是一种无接触的自动识别技术，利用射频信号及其空间耦合传输特性，实现对静态或移动待识别物体的自动识别，用于对采集点的信息进行“标准化”标识。鉴于RFID技术可实现无接触的自动识别，全天候、识别穿透能力强、无接触磨损，可同时实现对多个物品的自动识别等诸多特点，将这一技术应用到物联网领域，使其与互联网、通信技术相结合，可实现全球范围内物品的跟踪与信息的共享，在物联网“识别”信息和近程通讯的层面中，起着至关重要的作用。另一方面，产品电子代码(EPC)采用RFID电子标签技术作为载体，大大推动了物联网发展和应用。
![image-20231014132620913](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231014132620913.png)

基本工作方式是将RFID标签安装在被识别物体上（粘贴、嵌入、佩挂或植入等），当被识别物体进入读写器的读写范围内（射频场）时，标签与读写器之间建立联系，计算机后台利用读写器与RFID标签进行数据通信。



### You Can Clone but You Can’t Hide: A Survey of Clone Prevention and Detection for RFID

由于资源限制，大多数低成本标签不能支持哈希以外的复杂加密技术[14]。

RFID通信协议容易受到各种破坏，标签芯片易受到物理篡改

RFID的持续增长要求解决RFID应用带来的安全和隐私问题

#### 克隆攻击：

###### 克隆：

克隆攻击程序将受损标签中的数据**提取**到其他标签芯片或仿真设备中，称为克隆标签。电子产品代码(EPC)或标识符(ID)等标签数据可以在标签阅读器通信期间被窃听，而密钥等标签数据可以通过物理篡改来浏览，即使带有加密增强的标签也容易被克隆[17]。

###### 克隆得到：

克隆标签拥有其真正的对应物所期望拥有的**所有有效数据**，在参与RFID应用程序(如身份验证)时，克隆标签的行为与真正的标签相同。

###### 供应链[18]，入境卡[19]，[20]，车牌[21]，护照[16]，[22]，[23]，信用卡[24]，医药产品[25]，[26]

先进的加密技术可能有助于保护标签数据的保密性，但对于大多数正在使用的标签来说，这是负担不起的。因此，研究工作也致力于检测克隆标签。（先进加密技术成本高，所以致力于研究检测技术

###### 假设检测证据：大多数现有的检测方案都有一个隐含的假设，即真正的标签具有克隆标签无法获得的某些保密性。（这就变成假冒了

**挑战**：检测不能以标签更易受其他攻击为代价（加密标签ID有效防止克隆标签id，但为可追溯性攻击打开了大门[27]。可跟踪性攻击窃听标签发送的用于跟踪标签的固定消息

**<u>检测不是预防 （高估解决结果</u>**

预防：完全限制攻击者提取数据的机会

检测：接受标签易受到克隆攻击的事实

**<u>克隆不是假冒 （低估问题难度</u>**

假冒：id有效密钥无效

克隆：均有效

### 背景

#### RFID

![image-20231014165206431](C:\Users\Administrator\AppData\Roaming\Typora\typora-user-images\image-20231014165206431.png)

###### 服务器 

协议（身份识别、验证、定位 ） 数据库（记录标签包含的信息） 

服务器和阅读器通过有线/无线安全通道进行通信，负责管理和处理从RFID阅读器读取的数据

###### 阅读器 

阅读器和标签通过不安全的无线信道进行通信

- 与RFID标签通信的设备，它通过射频信号与标签交互。

- 阅读器会向标签发送请求，以激活标签并读取存储在标签上的信息。它可以通过射频天线发送和接收数据

###### 标签  

- 被附加到某些对象上，并装载与对象相关的数据

- 以射频信号的形式广播数据，允许RFID阅读器读取标签上的信息。

- 标签可以是被动标签（不具备自主能源）、半主动标签（具备自主能源，但无法主动发送数据）或主动标签（具备自主能源，可以主动发送数据）



#### 工作流程如下：

1. RFID阅读器与RFID标签通信：RFID阅读器通过射频信号与附近的RFID标签进行通信。阅读器可能发送请求以激活标签，并读取存储在标签上的信息。
2. 数据传输：当RFID标签被激活后，它会以射频信号的形式广播数据。RFID阅读器接收这些信号，并将数据传输到服务器。
3. 服务器处理数据：服务器接收来自阅读器的数据，并对数据进行处理、验证和分析。它可能与数据库进行交互，存储数据或执行业务逻辑。
4. 数据反馈：根据需要，服务器可以生成报告、向应用层软件发送数据，或将信息集成到企业级系统中。

#### RFID克隆

产出：包含真是标签所有有效数据的复制品

早期克隆通过窃听获得**低成本标签**：

1.只存储标签id，在读取器查询时以明文形式发出

2.阅读器仅通过标签ID的有效性来验证标签的真实性，即标签ID是否在数据库中

3.阅读器-标签通信是通过不安全的无线通道进行的(图2)，攻击者可以很容易地窃听传输中的标签id，并对其进行编程以克隆标签

4.缺乏读写器认证支持

进化：**物理篡改**

获取所有数据

无论是否加密，均可将其破解

**克隆初步尝试：**

###### 这些方法适用性、有效性均很有限

1.法拉第笼来屏蔽标签免受不必要的扫描 

法拉第笼：由信号屏蔽金属制成的容器

2.使用kill命令在销售点停用标签

并不能防止供应链中标签流通过程中的克隆攻击，使产品撤回和召回复杂化，且无法销毁复制品标签

###### 期待：RFID克隆对策有效 且不影响标签的可用性

##### 难点：

检测和预防

预防分为软、硬

软件：kill命令、反窃听、身份验证

硬件：物理层指纹等不可克隆的特性

相同的思想：身份验证和共享密钥、物理层指纹和电子指纹。



###### 预防不再赘述 RFID的克隆检测解决方案

#### 检测：

很难被完全阻止（说一下为什么预防难以实现？

**目的**：验证RFID系统中是否存在克隆标签

**分类**：P10

1. 集中式VS分布式

   克隆标签和真正的对应标签是否应该在同一系统中被检测到

2. 概率VS确定性

   概率检测用检测精度换取其他性能指标，如速度或通信开销

3. 可识别VS匿名

   需要已知id的RFID系统称为可识别系统。但是，标签id对于某些应用程序是隐私敏感的

4. 侦察VS识别

   通过检测，我们指的是更多地关注于验证克隆标签是否存在的解决方案。通过识别，我们指的是旨在检测系统中所有克隆标签的解决方案





