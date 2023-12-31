## A Survey on Encrypted Network Traffic Analysis

![image-20231028114630795](https://gitee.com/sygu66/img/raw/master/image-20231028114630795.png)

##### 加密与非加密流量识别

由于网络加密流量的比例不断提升，传统分类方法如基于端口、基于payload（有效负载）、基于主机行为等方法对网络流量分类的适用性大大降低。
由于加密流量几乎不包含任何内容可识别的模式特征，因此分类的第一步便是区分加密与非加密。

如今的主要思路是从信息熵来考虑，由于加密后的流量负载的随机性较强，信息熵大，可以将两者分开。


##### 加密网络特征选择

好的特征选取方法能令机器学习算法如虎添翼，但如何选取是一个难题。

网络流变化使得特征选择结果很难保持稳定，特征属性及其数目随之改变。
不同的特征选择方法缺少统一的评价指标。
因此，通过多种度量来综合选取高泛化能力的特征子集不失为一个有效的解决途径。
另外，在网络流量的特征提取方面，**时间维度**是一个比较重要的因素。

##### 加密流量识别问题的主要挑战

- 精细化分类是一个艰巨的任务。
- 数据多样导致模型对新数据的识别精度下降。

**加密流量自适应分类方法**
由于网络流随着时间、地域而不断变化，已经学习好的分类器对于新样本的泛化能力会比较差，但频繁更新分类器又是费时费力的，因此需要建立起一个自适应分类器，能不断检测网络流变化，并有效更新分类器。

然而收集无标签的数据非常容易，有标签的数据则是代价昂贵，因此考虑集成学习的方式，使用多种分类器增强泛化能力。

#### 未来研究方向

- 基于大数据的加密流量精细化识别
- 基于深度学习的层叠加加密流量识别
- 基于增量集成学习的协议变化自适应分类
- 基于区块链的伪装流量识别

![image-20231028124735563](F:\imgs\image-20231028124735563.png)

###### 粗粒度>------------------------------------------------->细粒度

**是否加密>协议>应用>服务>网页>异常流量>内容参数**

#### 识别对象等级

- 流级：关注流的特征和到达过程，还可分为单向流和双向流
- 包级：关注包的特征和到达过程，包的大小分布、到达时间、到达顺序等
- 主机级：关注主机间的连接模式，如主机（或端口）通信的所有流量
- 会话级：关注绘画的特征和到达过程，特征包括字节数和会话持续时间等

##### 2.1加密与未加密流量分类

首要任务。
Dorfinger:加密流量实时识别放大RT-ETD
Lotfollahi:基于神父学习的方法Deep Packet，将特征提取和分类阶段集成到一个系统中，不但能判断是否是加密流量，还能区分VPN和非VPN流量，网络模型为SAE（堆叠自编码器）和CNN。

##### 2.2 加密协议识别

加密协议识别由于各种协议封装格式不同，需要了解协议的交互过程，找出交互过程中的可用于区分不同应用的特征及规律才能总结出网络流量中各应用协议的最佳特征属性，最终为提高总体流识别的粒度和精度奠定基础。
加密协议交互过程大体可分为两个阶段

建立安全连接：握手、认证、密钥交换。在该过程中通信双方协商支持的加密算法，互相认证并生成密钥
第二阶段采用第一阶段产生的密钥加密传输数据
目前主流的加密协议有以下三种

###### 2.2.1 IPSec

一组确保IP层通信安全的协议栈，保护TCP/IP通信免遭窃听和篡改，其中包含了

认证头AH，为IP数据报提供无连接数据完整性、消息认证以及防重放攻击保护；
封装安全载荷ESP，提供机密性、数据源认证、无连接完整性、防重放和有限的传输流（traffic-flow）机密性；
安全关联SA，提供算法和数据包，提供AH、ESP操作所需的参数。
密钥协议IKE，提供对称密码的钥匙的生存和交换。
详细链接（百度百科）

###### 2.2.2 SSL/TLS

安全套接层协议SSL提供应用层和传输层之间的数据安全性机制，在客户端和服务器之间建立安全通道，对数据进行加密和隐藏，确保数据在传输过程中不被改变。SSL协议在应用层协议通信之前就已经完成加密算法和密钥的协商，在此之后所传送的数据都会被加密，从而保证通信的私密性。
详细链接（百度百科）

###### 2.2.3 SSH

安全外壳协议是一种在不安全网络上提供安全远程登录及其他安全网络服务的协议。主要包括传输层协议、用户认证协议和连接协议。
详细链接（百度百科）

##### 2.3 服务识别

服务识别就是识别加密流量所属的应用类型，如

流媒体：YouTube、优酷、土豆
SNS：Facebook、推特、微博
P2P：Skype、BitTorrent
随着P2P应用的不断发展，出现了多种混淆技术，如动态端口号、端口跳变、HTTP伪装、分块文件传输和加密有效载荷等，因此需要有效的方法来识别P2P流量。
Maiolini利用K-means进行分类，准确率达到99.8%

##### 2.4 异常流量识别

基于信息熵的异常检测算法比传统的流量分析提供更细粒度的识别。

Adami提出Skype-hunter，基于签名和流统计特征相结合的策略，能识别信令任务和数据业务

##### 2.5 内容参数识别

指对加密应用流量从内容属性上进一步识别，如视频清晰度及图片格式等。

Khakpour提出内容参数识别方法Iustitia


![image-20231028125218881](F:\imgs\image-20231028125218881.png)

![image-20231028183558277](F:\imgs\image-20231028183558277.png)