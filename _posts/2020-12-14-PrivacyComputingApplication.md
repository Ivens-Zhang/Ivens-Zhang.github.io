## 隐私计算的概念与应用现状

### 一、概念

**隐私计算（Privacy Computing）**是指在保护数据本身不对外泄露的前提下实现数据分析计算的一类信息技术，主要分为密码学和可信硬件两大领域。

隐私计算经过近几十年的发展，隐私计算技术的应用主要可以分为 **多方安全计算**、**可信硬件**、**联邦学习**三个主要流派。

**密码学的技术目前以多方安全计算（MPC）为代表。**多方安全计算(Secure Multi-Party Computation)是指在无可信第三方情况下，通过多方共同参与，安全地完成某种协同计算。即在一个分布式的网络中，每个参与者都各自持有秘密输入，希望共同完成对某个函数的计算，但要求每个参与者除计算结果外均不能得到其他参与实体的任何输入信息。也就是参与者各自完成运算的一部份，最后的计算结果由部分参与者掌握或公开共享。多方安全计算主要基于密码学的一些隐私技术，包括有同态加密(Homomorpgic Encryption)，不经意传输(Oblivious Transfer)，混淆电路(Garbled Circuit)，秘密共享(Secret Sharing)等。MPC 的核心思想是设计特殊的加密算法和协议，从而支持在加密数据之上直接进行计算。目前MPC通过秘密分割、不经意传输、混淆电路或同态加密等专门技术实现，通用性相对较低、性能处于中等水平，但近年来性能提升迅速、应用价值极高。

**可信硬件方面技术即通过硬件技术来对数据进行隔离保护。**可信执行环境 (TEE) 是 Global Platform (GP) 提出的概念。是移动设备主处理器上的一个安全区域，其可以保证加载到该环境内部的代码和数据的安全性、机密性以及完整性。TEE 提供一个隔离的执行环境，提供的安全特征包含：隔离执行、可信应用的完整性、可信数据的机密性、安全存储等。该技术的核心是企业和个人可以把数据处理模型部署在区块链上，在链下，例如 Intel SGX 可信执行环境中处理隐私数据，最终把可验证结果存储到链上并更新状态。

**此外，国内外还衍生出了联邦学习、共享学习、知识联邦、联邦智能等一系列“联邦学习类”技术。**这类技术以实现机器学习、数据建模、数据预测分析等具体场景为目标，通过对上述技术加以改进融合，并在算法层面进行调整优化而实现。

除了几种主流解决方案之外，还有差分隐私、K匿名算法、L多样性等隐私相关的技术，这些技术不是相互替代关系，而是可以相互结合、相互促进的。

### 二、详解多方安全计算

早在1982年，姚期智先生在其发表的文章《安全计算协议》（Protocols for Secure Computation）里提出了著名的姚氏百万富翁问题，同时也首次引入了双方安全计算的概念来解决问题，并对其可行性进行了验证。两个百万富翁Alice和Bob在无任何可信第三方，同时不暴露自己的财产的情况下，希望得出谁更富有的结论。为了解决这一问题，姚期智先生提出建立一个通用的框架处理单向函数所涉及的加密、完整性、智能扑克等系列问题，发展出了验证其安全性的通用技术。

时隔4年，姚期智先生再次取得重大突破，于1986年提出了基于混淆电路的通用解决方案，进一步验证了多方安全计算的通用可行性，同时也奠定了现代计算机密码学的理论基础。此后，经Oded Goldreich、Shafi Goldwasser等学者进一步的研究和创新，多方安全计算逐渐发展成为现代密码学的一个重要分支。

在实际应用中，通过综合运用密码学中混淆电路（Garbled Circuit）、不经意传输（Oblivious Transfer）、秘密分享（Secret Sharing）、同态加密（Homomorphic Encryption）、同态承诺（Homomorphic Commitment）、零知识证明（Zero-Knowledge Proof）等多种理论和协议，结合计算机工程技术研发出整套多方安全计算系统，能够使整个信息交互计算的过程全程在密文下进行，并且保证计算结果同明文下计算的结果一致。

我们可以先来思考一个问题：

> 两个百万富翁街头邂逅，他们都想比比谁更有钱，但是出于隐私，都不想让对方知道自己到底拥有多少财富，如何在不借助第三方的情况下，让他们知道他们之间谁更有钱?

这里假定：

- 两人都值得信任，不会作假
- 两人都希望诚实地比较出谁更服务（即谁的数更大）
- 两人又都希望知道对方财产到底是多少，如果可能的话，拿到具体数字最好了

其实这里假定的是一个安全多方计算的模型 - 半诚实对手模型，即计算方存在获取其他计算方原始数据的需求，但仍然按照计算协议执行。另外有恶意敌手模型，在这种模型中，参与方可以造假，即不按照计算协议执行计算过程。这就要复杂很多。为简化期间，本文仅讨论半诚实对手模型。

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

> **同态加密**（英语：**Homomorphic encryption**）是一种[加密](https://zh.wikipedia.org/wiki/加密)形式，它允许人们对密文进行特定形式的代数运算得到仍然是加密的结果，将其解密所得到的结果与对[明文](https://zh.wikipedia.org/wiki/明文)进行同样的运算结果一样。换言之，这项技术令人们可以在加密的数据中进行诸如检索、比较等操作，得出正确的结果，而在整个处理过程中无需对数据进行[解密](https://zh.wikipedia.org/wiki/解密)。其意义在于，真正从根本上解决将数据及其操作委托给第三方时的保密问题，例如对于各种[云计算](https://zh.wikipedia.org/wiki/云计算)的应用。
>
> 这一直是[密码学](https://zh.wikipedia.org/wiki/密码学)领域的一个重要课题，以往人们只找到一些部分实现这种操作的方法。而2009年9月[克雷格·金特里](https://zh.wikipedia.org/w/index.php?title=克雷格·金特里&action=edit&redlink=1)的论文[[1\]](https://zh.wikipedia.org/wiki/同态加密#cite_note-1)从数学上提出了“全同态加密”（英语：Fully homomorphic encryption）的可行方法，即可以在不解密的条件下对加密数据进行任何可以在明文上进行的运算，使这项技术获取了决定性的突破。人们正在此基础上研究更完善的实用技术，这对信息技术产业具有重大价值。

### 三、隐私计算的应用实例

随着跨系统、跨生态圈，甚至跨境信息交互的常态化，隐私信息未授权保存的问题越来越严重，给用户的隐私安全造成巨大威胁。以下以信息系统交互的四种模式为例，依次描述隐私计算框架下如何实现隐私保护，并在发生隐私侵犯行为时实现溯源取证。

我们以矩阵元开源的 [`Rosetta`](https://github.com/LatticeX-Foundation/Rosetta) 为例，在人工智能实训平台上部署并得出最后的结果：

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





### 四、未来研究方向

#### 4.1. 动态隐私度量

大型互联网企业等机构所控制的数据跨系统、跨境、跨生态圈流转，由于存在各种不同的数据类型和不同的应用场景，隐私度量的未来研究可以集中在三个方面：适合于多媒体场景下隐私信息的评估方法，隐私度量的动态调整机制，以及将隐私度量自动映射到约束条件和策略。通过解决大数据集动态隐私度量的核心问题可以支持场景自适应的隐私控制，特别是在大数据通过随机路径进行传播难以预测其流向的情况下。

#### 4.2. 隐私保护算法的基础理论

针对不同信息类型和隐私保护需求的隐私保护原子操作，需研究高效的隐私保护原语的基础理论。在基于加密的可逆隐私保护原语方面，重点在于全同态加密方法、部分同态加密算法、密文搜索、密文统计等密文计算理论。基于扰动的不可逆隐私保护原语方面，重点在于改进差分隐私模型并引入信息论中新的理论方法。

#### 4.3. 隐私保护效果评估

隐私保护算法的效果评估重点是要建立一套科学合理的量化体系，在这一量化体系指导下，对可逆和不可逆的隐私保护原语以及由原语的组合提出各对应指标的量化评估方法，包括隐私保护效果、数据可用性、算法复杂度等，以期为隐私保护方案的设计、比较和改进提供科学的评价依据。

#### 4.4. 隐私计算语言

研究隐私计算语言的语法体系，包括语句定义、编程接口、隐私保护原语的融合操作描述方法等，为复杂隐私保护方案的实现提供方便快捷、与硬件和操作系统等平台无关的编程工具，以支撑隐私保护机制在复杂互联信息系统中的实施部署。

#### 4.5. 隐私侵犯的判定准则与取证方法

在隐私计算框架对隐私信息进行描述的基础上，可以结合场景感知、隐私信息操作判定、隐私信息约束条件匹配等，对隐私侵权的多因素联合决策准则进行研究，从而确定决策的量化阈值。为了解决隐私侵犯事件发生后时空场景重构的关键问题，应该基于隐私信息描述中内嵌的取证信息、第三方监控与交叉多元素大数据分析，设计实用有效的取证方案。

### 五、结语

互联网、移动互联网、物联网等技术快速发展，并通过云服务汇聚数据，形成具有海量性、异构性等典型特征的大数据，为广大民众提供个性化服务，深刻地改变了人们的生产和生活方式。然而，信息服务却面临着收集、存储、共享、发布（含交换）、销毁等环节中的隐私信息泄露问题。

现有的各类解决方案趋于零散，尚未形成理论体系，本文所提出的隐私计算概念及其框架旨在建立全生命周期的隐私保护理论体系，包括隐私计算框架、隐私计算形式化定义、隐私计算应遵循的四个原则、算法设计准则、隐私保护效果评估与隐私计算语言。其中，隐私计算框架可支持跨平台的隐私信息交换、隐私信息流转的延伸授权、隐私侵犯的取证追踪；隐私计算语言（PCL）的设计目标是满足描述无歧义性、平台无关性和计算一致性，以支撑隐私保护的分层跨系统实施。最后，展望了隐私计算的研究发展趋势，期待隐私计算能够指引实用化的隐私保护技术研究，并指导大规模信息系统中隐私保护子系统的开发；同时，也期望隐私计算能为隐私保护标准的制定和隐私保护能力的评估提供理论支持。