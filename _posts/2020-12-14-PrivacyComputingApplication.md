## 隐私计算的概念与应用现状

> 根据IDC发布的数据，截止到2018年底，中国大数据解决方案市场软硬服总额达到388.8亿元人民币，并有望在2023年超过800亿元人民币，全球市场则将超过3000亿美元。

大数据时代，海量的数据的交叉计算和人工智能的发展为各行各业提供了更好的支持，但这些被使用的数据往往包含用户的隐私数据。所以这些数据往往是不对外开发，例如政府数据由于政策保密性完全不能对外公布，运营商、互联网公司收集到的客户数据，也不能透露给第三者，因此形成了一个个数据孤岛，数据之间不能互通，数据的价值无法体现。

如何应用海量的数据，实现数据流动，同时能够保护数据隐私安全、防止敏感信息泄露是当前大数据应用中的重大挑战。隐私计算就是为了解决这些问题应运而生。

### 一、概念

**隐私计算（Privacy Computing）**是指在保护数据本身不对外泄露的前提下实现数据分析计算的一类信息技术，主要分为密码学和可信硬件两大领域。

隐私计算经过近几十年的发展，隐私计算技术的应用主要可以分为 **多方安全计算**、**可信硬件**两大主要流派。此外，国内外还衍生出了联邦学习、共享学习、知识联邦、联邦智能等一系列“联邦学习类”技术，这类技术以实现机器学习、数据建模、数据预测分析等具体场景为目标，通过对上述技术加以改进融合，并在算法层面进行调整优化而实现。

**密码学的技术目前以多方安全计算（MPC）为代表。**多方安全计算(Secure Multi-Party Computation)是指在无可信第三方情况下，通过多方共同参与，安全地完成某种协同计算。即在一个分布式的网络中，每个参与者都各自持有秘密输入，希望共同完成对某个函数的计算，但要求每个参与者除计算结果外均不能得到其他参与实体的任何输入信息。也就是参与者各自完成运算的一部分，最后的计算结果由部分参与者掌握或公开共享。多方安全计算主要基于密码学的一些隐私技术，包括有同态加密(Homomorpgic Encryption)，不经意传输(Oblivious Transfer)，混淆电路(Garbled Circuit)，秘密共享(Secret Sharing)等。MPC 的核心思想是设计特殊的加密算法和协议，从而支持在加密数据之上直接进行计算。目前MPC通过秘密分割、不经意传输、混淆电路或同态加密等专门技术实现，通用性相对较低、性能处于中等水平，但近年来性能提升迅速、应用价值极高。

**可信硬件方面技术即通过硬件技术来对数据进行隔离保护。**可信执行环境 (TEE) 是 Global Platform (GP) 提出的概念。是移动设备主处理器上的一个安全区域，其可以保证加载到该环境内部的代码和数据的安全性、机密性以及完整性。TEE 提供一个隔离的执行环境，提供的安全特征包含：隔离执行、可信应用的完整性、可信数据的机密性、安全存储等。该技术的核心是企业和个人可以把数据处理模型部署在区块链上，在链下，例如 Intel SGX 可信执行环境中处理隐私数据，最终把可验证结果存储到链上并更新状态。

**此外，国内外还衍生出了联邦学习、共享学习、知识联邦、联邦智能等一系列“联邦学习类”技术。**这类技术以实现机器学习、数据建模、数据预测分析等具体场景为目标，通过对上述技术加以改进融合，并在算法层面进行调整优化而实现。

除了几种主流解决方案之外，还有差分隐私、K匿名算法、L多样性等隐私相关的技术，这些技术不是相互替代关系，而是可以相互结合、相互促进的。

### 二、详解多方安全计算

早在1982年，姚期智先生在其发表的文章《安全计算协议》（Protocols for Secure Computation）里提出了著名的姚氏百万富翁问题，同时也首次引入了双方安全计算的概念来解决问题，并对其可行性进行了验证。两个百万富翁Alice和Bob在无任何可信第三方，同时不暴露自己的财产的情况下，希望得出谁更富有的结论。为了解决这一问题，姚期智先生提出建立一个通用的框架，用以处理单向函数所涉及的加密、完整性、智能扑克等系列问题，该框架发展出了验证其安全性的通用技术。

时隔4年，姚期智先生再次取得重大突破，于1986年提出了基于混淆电路的通用解决方案，进一步验证了多方安全计算的通用可行性，同时也奠定了现代计算机密码学的理论基础。此后，经Oded Goldreich、Shafi Goldwasser等学者进一步的研究和创新，多方安全计算逐渐发展成为现代密码学的一个重要分支。

在实际应用中，通过综合运用密码学中混淆电路（Garbled Circuit）、不经意传输（Oblivious Transfer）、秘密分享（Secret Sharing）、同态加密（Homomorphic Encryption）、同态承诺（Homomorphic Commitment）、零知识证明（Zero-Knowledge Proof）等多种理论和协议，结合计算机工程技术研发出整套多方安全计算系统，能够使整个信息交互计算的过程全程在密文下进行，并且保证计算结果同明文下计算的结果一致。

我们可以先来思考一个问题：

> 两个百万富翁街头邂逅，他们都想比比谁更有钱，但是出于隐私，都不想让对方知道自己到底拥有多少财富，如何在不借助第三方的情况下，让他们知道他们之间谁更有钱?

这里假定：

- 两人都值得信任，不会作假
- 两人都希望诚实地比较出谁更富裕（即谁的数更大）
- 两人又都希望知道对方财产到底是多少，如果可能的话，拿到具体数字最好了

其实这里假定的是一个安全多方计算的模型 - 半诚实对手模型，即计算方存在获取其他计算方原始数据的需求，但仍然按照计算协议执行。另外有恶意敌手模型，在这种模型中，参与方可以造假，即不按照计算协议执行计算过程。这就要复杂很多。为简化起见，本文仅讨论半诚实对手模型。

有人提出这样一个解决方案：放一个天平，两边放上封闭的盒子，让两个富翁分别在两边放入质量相同的苹果，有几千万财富就放几个，最后看哪边重就可以了。

真的这么简单么？这里方案的提出者忽略了一个条件，也就是**不存在可信的第三方**，天平谁来提供？提供天平者是可以知道一切的。

这是一个看似简单实则非常复杂的问题。

**下面介绍姚期智先生的解：**

我们先把问题简化，假设两个百万富翁Alice和Bob各有钱i和j，i和j是1到10之间的数。

首先Bob挑选一个非常大的整数x，然后用Alice的公钥a加密。得到 k=Enc（x）。然后把这样一个数k-j+1发给Alice。Alice拿到这个数之后呢，计算以下这些数：

```
Dec{k-j+1, k-j+2, k-j+3 ...., k-j+10}
```

然后除以一个素数p取余得到：

```
z1 = Dec{k-j+1}(mod p), z2, z3, .... z10
```


因为Alice的钱是i，那么Alice 做一下这个事情：

```
z1, z2, z3, ... Zi, Z(i+1) = Z(i+1)+1
```


然后把这一串数字发给Bob，Bob只需要看第j个数字，如果等于x（mod p）就说明i>=j，否则说明i<j，然后`bob`把结果返给`Alice`即可。

**多方安全计算使用到的隐私技术：**

多方安全计算主要基于密码学的一些隐私技术，包括有：

- 秘密共享(Secret Sharing)
- 混淆电路(Garbled Circuit)
- 不经意传输(Oblivious Transfer)
- 同态加密(Homomorpgic Encryption)

##### 秘密共享

秘密共享(Secret-Sharing) 是现代密码学领域的一个重要分支，是信息安全和数据保密中的重要手段，也是多方安全计算和联邦学习等领域的一个基础应用技术。实际应用中，在密钥管理，数字签名，身份认证，多方安全计算，纠错码，银行网络管理以及数据安全等方面都有重要作用。

秘密共享是在一组参与者中共享秘密的技术，它主要用于保护重要信息，防止信息被丢失、被破坏、被篡改。它源于经典密码理论，最早由Sharmir和Blakley在1979年提出。简单来说，秘密共享就是指共享的秘密在一个用户群体里进行合理分配，以达到由所有成员共同掌管秘密的目的。

基于Shamir秘密共享理论的方法中，秘密共享的机制主要由秘密的分发者D、团体参与者P{P1，P2，…，Pn}、接入结构、秘密空间、分配算法、恢复算法等要素构成。

秘密共享通过把秘密进行分割，并把秘密在n个参与者中分享，使得只有多于特定t个参与者合作才可以计算出或是恢复秘密，而少于t个参与者则不可以得到有关秘密。

<img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/20201020101716.png" style="zoom: 80%;" />

<center>单分发者秘密共享机制</center>

秘密共享体系还具有同态的特性。如下图所示有特征A和B，他们的值被随机分成碎片(X1, X2, …, Xn)和(Y1, Y2, …, Y3)，并分配到不同参与节点(S1,S2, …, Sn)中，每个节点的运算结果的加和能等同于原始A与B的加和。同样通过增加其他计算机制，也能满足乘积的效果，这就是秘密共享具备的“同态性”，各参与者可以在不交换任何数据的情况下直接对密码数据求和，乘积。

<img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/20201020101822.png" style="zoom:80%;" />

<center>多分发者秘密共享机制</center>

在秘密共享系统中，攻击者必须同时获得一定数量的秘密碎片才能获得密钥，通过这样能提高系统的安全性。另一方面，当某些秘密碎片丢失或被毁时，利用其它的秘密份额仍让能够获得秘密，这样可提高系统的可靠性。

秘密共享的上述特征，使得它在实际中得到广泛的应用，包括通信密钥的管理，数据安全管理，银行网络管理，导弹控制发射，图像加密等。

##### 混淆电路

混淆电路(Garbled Circuit)是姚期智教授[4]在80年代提出的安全计算概念。通过布尔电路的观点构造安全函数计算，达到参与者可以针对某个数值来计算答案，而不需要知道他们在计算式中输入的具体数字。

在这里关键词是“电路”，实际上所有可计算问题都可以转换为各个不同的电路，例如加法电路，比较电路，乘法电路等。而电路是由一个个门（gate）组成，例如与门，非门，或门，与非门等。

混淆电路里的多方的共同计算是通过电路的方式来实现，例如下图所示，Alice和Bob要进行多方计算，他们首先需要构建一个由与门，或门，非门，与非门组成的布尔逻辑电路，每个门都包括输入线，输出线。

<img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/20201020101925.png" style="zoom:80%;" />

混淆电路则通过加密和扰乱这些电路的值来掩盖信息，而这些加密和扰乱是以门为单位，每个门都有一张真值表。

<img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/20201020101927.png" style="zoom:67%;" />

Alice用密钥加密真值表，并把表打乱后发给Bob，通过这种这加密+打乱的过程，达到混淆电路的目的。而Bob在接收到加密表后，根据收到的加密真值表，混淆的输入，及自己的Key，对加密真值表的每一行尝试解密，最终只有一行能解密成功，并提取相关的加密信息。最后Bob将计算结果返回给Alice。

在整个过程大家交互的都是密文或随机数，没有任何有效信息泄露，在达到了计算的目的，同时达到了对隐私数据数据保护的目的。

##### 不经意传输

不经意传输(Oblivious Transfer - OT)最早在1981年被 Michael O. Rabin提出，之后被广泛应用于多方安全计算等领域。

在Rabin [1] 的OT协议中，发送者Alice发送一个信息m给接收者Bob，接收者Bob以1/2的概率接受信息m。所以在协议交互的结束的时候，发送者Alice并不知道Bob是否接受了消息，而接收者Bob能确信地知道他是否得到了信息m，从而保护了接收者的隐私性，同时保证了数据传输过程的正确性。该方法主要是基于RSA加密体系构造出来。

1985年S. Even, O. Goldreich, and A. [lem](https://36kr.com/projectDetails/542543)pel [2] 提出了1-out-2 OT, 在此方案中发送者Alice每次发送2个信息和，而接收者Bob每次输入一个选择b, 当协议结束的时候，发送者Alice无法获得关于接收者Bob的任何有价值的信息，而接收者Bob只能获得，对于, 接收者Bob也一无所知。

<img src="https://gitee.com/zhangyi98/pictureBed/raw/master/img/20201020102100.png" style="zoom: 67%;" />

在1986年，Brassard等人将1-out-2 OT扩展为1-out-n OT。

![](https://gitee.com/zhangyi98/pictureBed/raw/master/img/20201020102102.png)

在实际应用中，不经意传输OT的一种实施方式是基于RSA公钥加密技术。

一个简单的实施流程如下：

1. 首先，发送者生成两对不同的公私钥，并公开两个公钥，称这两个公钥分别为公钥1和公钥2。假设接收人希望知道m1，但不希望发送人知道他想要的是m1。
2. 接收人生成一个随机数k，再用公钥1对k进行加密，传给发送者。
3. 发送者用他的两个私钥对这个加密后的k进行解密，用私钥1解密得到k1，用私钥2解密得到k2。显然，只有k1是和k相等的，k2则是一串毫无意义的数。但发送者不知道接收人加密时用的哪个公钥，因此他不知道他算出来的哪个k才是真的k。
4. 发送人把m1和k1进行异或，把m2和k2进行异或，把两个异或值传给接收人。显然，接收人只能算出m1而无法推测出m2（因为他不知道私钥2，从而推不出k2的值），同时发送人也不知道他能算出哪一个。

##### 同态加密

同态加密(Homomorphic Encryption)是一类具有特殊属性的加密方法，是Ron Rivest, Leonard Adleman, 以及Michael L. Dertouzo在1978年提出的概念。与一般加密算法相比，同态加密除了能实现基本的加密操作之外，还能实现密文间的多种计算功能，即先计算后解密可等价于先解密后计算。这个特性属性对于保护信息的安全具有重要意义，利用同态加密技术可以先对多个密文进行计算之后再解密，不必对每一个密文解密而花费高昂的计算代价；利用同态加密技术可以实现无密钥方对密文的计算，密文计算无须经过密钥方，既可以减少通信代价，又可以转移计算任务，由此可平衡各方的计算代价；利用同态加密技术可以实现让解密方只能获知最后的结果，而无法获得每一个密文的消息，可以提高信息的安全性。

同态加密主要分两类：

- 全同态加密(Fully Homomorphic Encryption)：全同态加密同时满足同态加法运算和同态乘法运算。这意味着同态加密方案支持任意给定的f函数，只要这个f函数可以通过算法描述，就可以用计算机实现。但全同态计算开销极大，暂时还无法在实际中使用。
- 部分同态加密(Somewhat Homomorphic Encryption )：部分同态加密只支持同态加法运算和数乘运算，这意味着此同态加密方案只支持一些特定的f函数。但部分同态加密也意味着开销会变得较小，容易实现，现在已经可以在实际中使用。

目前满足加法同态和数乘同态的算法包括Paillier和Benaloh算法等，而满足乘法同态的算法包括RSA和ELGamal算法等。

同态加密技术在分布式计算环境下的密文数据计算方面具有比较广泛的应用领域，比如安全云计算与委托计算、多方保密计算、匿名投票、文件存储与密文检索等。例如在云计算方面，虽然目前云计算应用中，从安全角度来说，用户还不敢将蜜柑信息直接放到第三方云上进行处理，通过实用的同态加密技术，则大家可以放心使用各种云服务，同时各种数据分析过程中也不会泄露用户隐私。加密后的数据在第三方服务处理后得到加密后的结果，这个结果只有用户自身可以进行解密，整个过程第三方平台无法获知任何有效的数据信息。另外一个应用，在区块链上，使用同态加密技术，智能合约也可以处理密文，而无法获知真实数据，能极大的提高隐私安全性。

### 三、隐私计算的应用实例

随着跨系统、跨生态圈，甚至跨环境交互的常态化，隐私信息未授权保存的问题越来越严重，给用户的隐私安全造成巨大威胁。以下以信息系统交互的四种模式为例，依次描述隐私计算框架下如何实现隐私保护，并在发生隐私侵犯行为时实现溯源取证。

我们以矩阵元开源的 [`Rosetta`](https://github.com/LatticeX-Foundation/Rosetta) 为例，在人工智能实训平台上部署并得出最后的结果：

<img src="https://gitee.com/zhangyi98/pictureBed/raw/master//img/20201225100744.png" style="zoom:50%;" />

#### 3.1. 安装依赖 & 部署

成功部署 `Rosetta` 需要以下依赖：

```bash
Ubuntu (18.04=)
Python3 (3.6+)
Pip3 (19.0+)
Openssl (1.1.1+)
TensorFlow (1.14.0=, cpu-only)
CMake (3.10+)
Rosetta (latest)
```

- **Ubuntu**:
  Check version:

  ```
  lsb_release -r         # e.g. Release: 18.04
  ```

  > ***Note: If your OS version is less than 18.04, then you should upgrade your operating system and then continue the following steps***

- **Python3 & Pip3 & Openssl & CMake** Check the version:

  ```
  python3 --version     # e.g. Python 3.6.9
  pip3 --version        # e.g. pip 20.0.2
  apt show libssl-dev   # e.g. Version: 1.1.1-1ubuntu2.1~18.04.5
  cmake --version       # e.g. cmake version 3.15.2
  ```

  If the above software versions are not met, you may install or upgrade them as follows:

  ```
  # install python3, pip3, openssl
  sudo apt update
  sudo apt install python3-dev python3-pip libssl-dev cmake
  # upgrade pip3 to latest 
  sudo pip3 install --upgrade pip
  ```

- **TensorFlow**

  ```
  # Optional, to depress the warning of tensorflow
  pip3 install numpy==1.16.4 --user
  # install tensorflow
  pip3 install tensorflow==1.14.0 --user
  ```

- **Rosetta**

  ```
  # clone rosetta git repository
  git clone https://github.com/LatticeX-Foundation/Rosetta.git --recursive
  # compile, install and run test cases
  cd Rosetta && bash compile_and_test_all.sh
  ```

等待脚本编译、运行完成后我们可以开始多个节点的部署。

#### 3.2. 证书、脚本

为三个计算节点`P0`、`P1`、`P2`分别创建工作目录，比如: `millionaire0`、`millionaire1`、`millionaire2`

```
mkdir millionaire0 millionaire1 millionaire2
```

- 下载百万富翁问题范例

下载范例[Python程序](https://github.com/LatticeX-Foundation/Rosetta/blob/master/example/millionaire/millionaire.py) 到`millionaire0`、`millionaire1`、`millionaire2`工作目录.

```
wget https://github.com/LatticeX-Foundation/Rosetta/tree/master/example/millionaire/millionaire.py
```

```py
# millionaire.py
#!/usr/bin/env python3

# import rosetta package
import latticex.rosetta as rtt
import tensorflow as tf

# activate protocol, here use SecureNN.
rtt.activate("SecureNN")

# get private data from console
Alice = tf.Variable(rtt.private_console_input(0))
Bob = tf.Variable(rtt.private_console_input(1))

# define comparsion operation
res = tf.greater(Alice, Bob)

# run computation
with tf.Session() as sess:
    sess.run(tf.global_variables_initializer())
    res = sess.run(res)

    # get the result and output
    print('ret:', sess.run(rtt.SecureReveal(res)))  # ret: 1.0
```

- 生成证书和key `P0`、`P1`、`P2`分别生成ssl服务端证书和key，执行命令:

```
mkdir certs
# generate private key
openssl genrsa -out certs/server-prikey 4096
# if ~/.rnd not exists, generate it with `openssl rand`
if [ ! -f "${HOME}/.rnd" ]; then openssl rand -writerand ${HOME}/.rnd; fi
# generate sign request
openssl req -new -subj '/C=BY/ST=Belarus/L=Minsk/O=Rosetta SSL IO server/OU=Rosetta server unit/CN=server' -key certs/server-prikey -out certs/cert.req
# sign certificate with cert.req
openssl x509 -req -days 365 -in certs/cert.req -signkey certs/server-prikey -out certs/server-nopass.cert
```

> 注意: 实环境部署，建议使用第三方可信证书。

编写配置文件，逐个放入 `millionaire0/ 、millionaire0/ 、 millionaire0/` 中，配置文件模版如下:

```json
{
  "PARTY_ID": 0,
  "MPC": {
    "FLOAT_PRECISION": 16,
    "P0": {
      "NAME": "PartyA(P0)",
      "HOST": "127.0.0.1",
      "PORT": 11121
    },
    "P1": {
      "NAME": "PartyB(P1)",
      "HOST": "127.0.0.1",
      "PORT": 12144
    },
    "P2": {
      "NAME": "PartyC(P2)",
      "HOST": "127.0.0.1",
      "PORT": 13169
    },
    "SAVER_MODE": 7,
    "SERVER_CERT": "certs/server-nopass.cert",
    "SERVER_PRIKEY": "certs/server-prikey",
    "SERVER_PRIKEY_PASSWORD": ""
  }
}
```

字段说明:

- `PARTY_ID`: 计算节点参与的角色ID，可取0，1，2，分别对应`P0`、`P1`、`P2`
- `MPC`: 指定为安全多方计算协议配置
- `FLOAT_PRECISION`: 安全多方浮点计算的精度位数
- `P0`、`P1`、`P2`: 别为三方`MPC`联合训练player `P0`，`P1`，`P2`
- `NAME`: `MPC` player名字标识
- `HOST`: 主机地址
- `PORT`: 通信端口
- `SERVER_CERT`: 服务端签名证书
- `SERVER_PRIKEY`: 服务端私钥
- `SERVER_PRIKEY_PASSWORD`: 服务端私钥密码口令（没有设置则为空字符串）
- `SAVER_MODE`: 模型保存配置值，可以通过此值的配置设定保存的模型中的参数值是否为明文值，或者在具体哪一参与方中保存为明文，具体请参考[算子API文档](https://github.com/LatticeX-Foundation/Rosetta/blob/master/doc/API_DOC_CN.md)。

#### 3.3. 运行

`P0`、`P1`、`P2`分别在`millionaire0`、`millionaire1`、`millionaire2`目录下进行测试，使用模版配置并保存为CONFIG.json。

运行`百万富翁问题`范例:

> 注意：运行过程将提示控制台输入值

- **`P0`节点**

```
mkdir log
# MPC player 0
python3 millionaire.py --party_id=0
```

![](https://gitee.com/zhangyi98/pictureBed/raw/master//img/20201222173517.png)

- **`P1`节点**

```
mkdir log
# MPC player 1
python3 millionaire.py --party_id=1
```

![](https://gitee.com/zhangyi98/pictureBed/raw/master//img/20201222173533.png)


- **`P2`节点**

```
mkdir log
# MPC player 2
python3 millionaire.py --party_id=2
```

![](https://gitee.com/zhangyi98/pictureBed/raw/master//img/20201222173545.png)

最后，看到这样的输出:

```
-------------------------

[2020-07-29 20:10:49.070] [info] Rosetta: Protocol [SecureNN] backend initialization succeeded!

please input the private data (float or integer, 6 items, separated by space): 2 3 1 7 6 2

-------------------------

plaintext matmul result: [[b'8.000000' b'14.000000' b'18.000000' b'4.000000'] [b'4.000000' b'7.000000' b'9.000000' b'2.000000'] [b'24.000000' b'42.000000' b'54.000000' b'12.000000']]

[2020-07-29 20:11:06.452] [info] Rosetta: Protocol [SecureNN] backend has been released.

-------------------------

plaintext matmul result: [[b'0.000000' b'0.000000' b'0.000000' b'0.000000'] [b'0.000000' b'0.000000' b'0.000000' b'0.000000'] [b'0.000000' b'0.000000' b'0.000000' b'0.000000']]

[2020-07-29 20:11:06.452] [info] Rosetta: Protocol [SecureNN] backend has been released.

-------------------------

运行成功：1.0

-------------------------
```

参考矩阵元官方仓库中[实例说明文档](https://github.com/LatticeX-Foundation/Rosetta/tree/master/example/millionaire)，结果输出 `ret: 1.0` 则表示单机部署测试成功。

<img src="https://gitee.com/zhangyi98/pictureBed/raw/master//img/20210108101339.png" style="zoom: 67%;" />

### 四、未来研究方向

#### 4.1. 动态隐私度量

大型互联网企业等机构所控制的数据跨系统、跨环境、跨生态圈流转，由于存在各种不同的数据类型和不同的应用场景，隐私度量的未来研究可以集中在三个方面：适合于多媒体场景下隐私信息的评估方法，隐私度量的动态调整机制，以及将隐私度量自动映射到约束条件和策略。通过解决大数据集动态隐私度量的核心问题可以支持场景自适应的隐私控制，特别是在大数据通过随机路径进行传播难以预测其流向的情况下。

#### 4.2. 隐私保护算法的基础理论

针对不同信息类型和隐私保护需求的隐私保护原子操作，需研究高效的隐私保护原语的基础理论。在基于加密的可逆隐私保护原语方面，重点在于全同态加密方法、部分同态加密算法、密文搜索、密文统计等密文计算理论。基于扰动的不可逆隐私保护原语方面，重点在于改进差分隐私模型并引入信息论中新的理论方法。

#### 4.3. 隐私保护效果评估

隐私保护算法的效果评估重点是要建立一套科学合理的量化体系，在这一量化体系指导下，对可逆和不可逆的隐私保护原语以及由原语的组合提出各对应指标的量化评估方法，包括隐私保护效果、数据可用性、算法复杂度等，以期为隐私保护方案的设计、比较和改进提供科学的评价依据。

#### 4.4. 隐私计算语言

研究隐私计算语言的语法体系，包括语句定义、编程接口、隐私保护原语的融合操作描述方法等，为复杂隐私保护方案的实现提供方便快捷、与硬件和操作系统等平台无关的编程工具，以支撑隐私保护机制在复杂互联信息系统中的实施部署。

#### 4.5. 隐私侵犯的判定准则与取证方法

在隐私计算框架对隐私信息进行描述的基础上，可以结合场景感知、隐私信息操作判定、隐私信息约束条件匹配等，对隐私侵权的多因素联合决策准则进行研究，从而确定决策的量化阈值。为了解决隐私侵犯事件发生后时空场景重构的关键问题，应该基于隐私信息描述中内嵌的取证信息、第三方监控与交叉多元素大数据分析，设计实用有效的取证方案。

### 五、结语

在科技改变生活、网络引领未来的新时代，世界信息技术革命日新月异，信息化和经济全球化相互促进，互联网已经融入社会生活方方面面，隐私计算研究范畴及发展改变了人们的生产和生活方式。随着移动互联网、云计算和大数据技术的发展，数据的收集、共享、发布与分析会导致用户隐私信息的泄露，给用户带来巨大的损失。现有的各类隐私保护机制和方法并没有实现对隐私进行科学、系统和定量化地刻画，使隐私保护方法的研究缺乏理论指导，呈现碎片化的趋势。个人隐私会同时存在不同信息系统中，客观上这些不同信息系统虽然实现了不同等级的安全保护，但某一信息系统的泄露将导致其他信息系统的隐私保护的失效，因此如何确保存有用户隐私信息的所有信息系统达到同等安全级别成为一个难题。本文介绍了隐私计算的基本概念，对姚期智先生提出的百万富翁问题进行了详细解释并使用 [`rosetta`](https://github.com/LatticeX-Foundation/Rosetta) 进行实践部署，最后对隐私计算研究的发展趋势进行了展望，期待能促进未来建立科学、系统的隐私保护理论与技术体系。