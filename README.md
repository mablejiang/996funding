
# 为996设计一个虚拟货币捐款模型



## 为什么要做这样一个模型

996.ICU代表着一类新型社会组织 — 自发，开源，匿名，去中心，临时为某一共同目标迅速组成的协作组织。这样势不可挡的线上组织虽然利用github，stack等平台工具迅速取得了很多进展，获得了巨大的影响力，但依然有它的软肋。

**这个软肋就是资金**一旦涉及到资金，问题就变得复杂。钱让这个简单的组织变得不纯粹，但没有钱，很多事情都只能是媒体宣传或纸上谈兵，难以解决真实的问题。说句实话，现在哪个普通员工按照劳动法跟996的公司打个官司，都不一定出得起诉讼费，更不要说还搭上之后找不到工作的隐性成本。所以，一定的经济激励是合理的，尤其对于鼓励先行者。

现实世界留给我们的选择十分有限：

1. **成立海外NGO组织**，并且严格遵守NGO的收款，审计规定，定期公开财务数据。
2. 创始人的**私人账号**，比如paypal, patron, 海外银行账号。
3. **虚拟货币地址**，私钥掌握在筹款者的手中。

针对996这样匿名，时效性又很重要的“临时组织”，NGO的成本是极高的，而且需要的时间太长，根本跟不上媒体关注和主动捐款意愿的速度。

对于方法2，风险有两个，一个是创始人本身的信用风险。如果财务数据不够透明，很容易引起社区内部的不信任甚至互撕，因为开源项目的贡献是大家的，而钱只打给了一个人。第二是接受者**必须一定程度地暴露自己的身份**。如果官方想查到你是谁，那是轻而易举的，人肉搜索也不在话下。

方法3的好处是可以保证捐款人和被捐款人的完全匿名，而且财务数据完全透明，大家可以看到从开始募集到的金额以及已经花费的数目和去处。但是，因为私钥必须掌握在一个特定的人手中，所以资产托管还是中心化的。收到捐款，决定捐款去向，都是中心化决定的，就算引入投票机制也无法避免买票，作假等行为，做不到完全公平透明。

有没有可能在相对更好的方法3的基础上，通过改进经济机制，设计出更合理的捐款模型呢？

我们的目标是：
— 财务公开透明
— 资金安全
— 捐款用途尽量可追踪，民主化
— 支持小额，分散的捐款
— 支持匿名收款和捐款

在无法提出完美解决方案的阶段下，我们可以无限接近上述目标，并尽量减小，而不是消除中心化的风险。

受到许多虚拟货币和区块链领域的思想启发，我们设计了一个简单的经济模型，并尽量做到技术上的可实现。




## 最简单的设计

利用bancor协议，我们可以轻松实现一个可自由兑换的捐款系统。对于bancor模型和实现方式，我们接下来具体解释，详细见[BES内部深度 - Bancor算法及IBO的未来](https://mp.weixin.qq.com/s/Cr33JNFLdY6YJjiw4yhcmg)。先来抽象化一下模型：

* **发行代币**：在以太坊上发行一个新的ERC20代币，例如叫996.token

* **生成账户**：利用Bancor协议生成一个连接器（connector）

* **确定数量**：确定bancor算法的重要参数，存入一定量的ETH进入储备金池（connector balance），存入一定量的996.token供给（supply)

![](https://github.com/yujadehou/996funding/blob/master/image/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-22%20%E4%B8%8B%E5%8D%885.45.14.png)

Bancor协议可以实现更多复杂的数学设计及应用场景。在这里，我们设定CW=100%=1，以保证捐款是一个没有投机的行为。那么存入储备金池的ETH，实际上就是在给996.token定价。

**但请注意，这个定价是没有实际意义的，因为CW=1，在连接器没有变动的情况下，价格永远维持不变，只是一个纯粹的数字。**连接器变化的情况，见下文讨论，但只会让定价变低，不会变高。

例如，我们存入连接器中的ETH是100个，而996.token有100万，CW为1，那么代币价格就为price = 100/1000000 = 0.00001ETH 

为什么设定CW=1呢？因为只有在CW=1的情况下，随着捐款数量（supply）的变化，token的价格才会保持不变。

![](https://github.com/yujadehou/996funding/blob/master/image/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-22%20%E4%B8%8B%E5%8D%885.45.24.png)


* **接受捐款**：捐款者直接把ETH存入连接器中，并以上面参数设定的比例兑换处996的token。现在，与传统的捐款模型最大的区别来了：

**你随时可以通过兑换token取回部分捐款。**由于连接器是一个随时自由兑换的交易对手，你可以在任何时候用自己手中的token**以原价格或更低的价格**兑换回ETH。这是一个比传统捐款多出的选择。

* **取走捐款**：

如果管理者取走捐款是不定期，分批的，**那么每一次取走都会影响token的定价使其被稀释**。但由于我们的场景是捐款而不是投资，价格对捐款者的影响很小。取走捐款的方式有两种：

1. 直接部分取出连接器（也就是账户）中的ETH。
2. 增发token放入连接器，再用token自由兑换ETH并取出 

所以，要么储备金ETH变少，要么token增多并被兑换。他们的影响是相同的：

1. 管理者取出了部分资金
2. 996.token的价格被稀释
例如，取走20%的ETH资金，那么token的价格也会被稀释到80%

也就是说，token的定价与最开始相比，只可能越来越低。这个速度会被用款速度决定，而不受捐款速度影响。只要资金池没有被全部取出，捐款者就还是可以取回部分捐款，但可能是以被稀释的价格。

至此，这个连接器，也是一个以太坊上的公钥地址，也就成了“捐款账户”。所有的捐款时间和来源，支出时间和去向都会透明地记录在这个账簿上。只要资金还没有被管理者取走，捐款者也可以在任何时候追加或部分取回捐款。例如，在看到短时间内捐款迅速积累，觉得社区资金已经充足的时候，可以拿回早期捐款。


_这个模型是好处是什么呢？_

— **适合小额，分散的捐款**

— **可以始终保持匿名**

— **捐款者对管理者的监督更强**：如果管理者在短时间内拿走所有的钱甚至跑路，那后来的捐款者会看到，对管理者的信任降低，停止捐款。而如果管理者以某种程度合理，公开地运用捐款，捐款者也可以在任何时候追加或部分取回捐款。例如，在看到短时间内捐款迅速积累，觉得社区资金已经充足的时候，可以拿回早期捐款。

—  **参与治理**：现在的模型中，996.token本身是没有实际作用的，更像一个“收据”。但是，如果引入token的社区治理，996.token就可以用来投票。例如核心社区成员手中的token有80%，捐款者20%，可以联合起来对重大决议，或用款项目进行投票，超过51%才能拨款。

— **便于分叉**：由于开源项目是开放式贡献的，发起人并不代表一定会一直是社区核心成员，也不一定是募资的发起人和管理人。利用虚拟货币模型的优点是可以分叉。一旦社区成员或捐款者对于资金使用等问题有质疑，不能解决的，可以直接分叉，开启一个新的捐款地址。只要你有能力让别人相信你，社区甚至可以有多个“中心”，规避了单一的黑天鹅风险。




## 更具体的探讨

上述是最简单的模型，几乎没有任何数学和技术上的挑战。其他更复杂的功能或更具体的讨论呢？

* 是否能引入奖励机制？

在这里，为了保证项目的“纯洁性”，我们从技术设计上规避了投机的可能。但实际上由于bancor协议本身是一个为项目募资的通用模型，所以是可能实现价格发现机制的。

当CW<100%的时候，连接器的定价变成了杠杆机制，随着捐款（supply）的增多，价格会越来越高。而如果更多人卖出，supply下降，价格会再次回落。

举个栗子，假设996目前有一个特殊的项目需要捐款。项目发起人A，社区参与者B，捐款者C。A自己在最早期投入1个ETH，获得100个996.token。C1投入1ETH获得80个996.token。A组织了10位B开始了这项活动，每位志愿者分得5个token，B总共分得50个token。C2继续为活动赞助了3 ETH获得120个token。此时，A可以兑现手中剩余的token换回最开始投入的1ETH，B也可以选择兑换。
![](https://github.com/yujadehou/996funding/blob/master/image/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-22%20%E4%B8%8B%E5%8D%885.45.33.png)

曲线也可以是这样的：

![](https://github.com/yujadehou/996funding/blob/master/image/%E5%B1%8F%E5%B9%95%E5%BF%AB%E7%85%A7%202019-04-22%20%E4%B8%8B%E5%8D%885.45.39.png)
右图的二级函数既能激励早期参与者和捐款者，又能避免后来者的投机心理。

这种更复杂的设计的好处是可以让整个社区不单纯依赖外部捐款，而变成了自组织，早期贡献者和捐款者都得到了激励。坏处是复杂，而且可能引入动机不纯的投机行为，污染整个社区。

鉴于996是一个本身就不成熟的实验性组织，**目前不建议引入这种在行为动机，经济学设计，规则设计都过于复杂的机制**，但开开脑洞总是好的。详见[BES 内部深度 - 什么是联合曲线模型](https://mp.weixin.qq.com/s/uFihdviMm6O30GAvzkwdzw)


* 如何避免大公司通过钱来控制组织？

刚才说到，token可以用来投票，作为线上社区治理的工具。那如果大公司，比如在这次事件中备受争议的BAT，通过匿名方式购买大量token，影响投票结果呢？

我认为这个是可以通过设计或分叉来解决的。从设计上，token不仅可以通过兑换获得，也可以中心化分配到核心贡献者手上。前提是透明，有预期地数量分配，并不随意修改规则。

从分叉上，如果大公司出于邪恶目的垄断了捐款和票池，那么社区完全可以分叉出新的投票地址和bancor曲线。因为所有信息都是对称的，公平靠整个社区的监督和投票来维持。


* 能否采用比特币或其他货币？

由于比特币没有智能合约，目前上述bancor协议只能在ETH或EOS上实现，并且已经有相对成熟的基础设施。如果对协议本身不满意，也可以自己分叉一个新的bancor，反正代码很简单。

但如果社区想接受比特币捐款，或者希望把接收到的ETH先兑换成价值更稳定的代币，可以采用一些更迂回的方法，或直接使用USDT等稳定币。虽然最常见的匿名捐款目前还是比特币，但社区成员也可以通过二次兑换等折中的方法最后连接到我们目前的智能合约捐款模型。


* 如何解决private key的私有问题？

刚才提到连接器中取走捐款，必须由掌握私钥的人进行，而私钥（private key）只能掌握在一个人手里。这就注定是中心化的。

但是，我们的目的是为了和现有的前文提到的1，2，3种选择相比，而不是一个完美的模型。虽然私钥是中心化的，但整个过程已经比传统的捐款更透明，更受监督，也更容易分叉。

不过，在机制上确实可以更有趣地帮助解决这个问题。例如，如果996社区本来就不希望成为一个长期运营的NGO组织，而只是在有明确目的的时候需要捐款，那我们可以把上述整个模型作为单个预算项目发起。

假如A是一个在某地起诉公司的员工，希望募集他的律师费；B是工程师，希望把996的证据付费存储在各个云上，或自建服务器；C是码农，希望组织团队把我们文章的经济模型做成可以使用的产品。

那么他们可以分别发起捐款申请，而每个申请是一个单独的bancor曲线和公钥地址，私钥在发起人手中。他们不仅可以设定适合自己的曲线参数，还能设定捐款上限，保证小型，分散，安全。社区管理者只是帮助审核预算的真实性，和信用背书，号召捐款。捐款者可以选择支持他们认为有意义的用途，更多地参与社区治理。

这样，整个社区就更像一个自治组织（DAO），而不是依赖外部捐款的NGO。




## 还能更有想象力吗

好了，现在我们的意图已经十分明显：**建立一个适合于所有非营利组织的虚拟货币捐款模型**，而非只适用于996。

资金并不是他们唯一的困境，但这个问题足够通用，足够普遍。另外一方面，虚拟货币发展到今天，最大的问题依然是没有应用场景。已经有无数的实验和产品发生在投资，投机，交易领域，但就算包括比特币在内，虚拟货币也没有证明自己给世界带来什么实在的价值。或者说，启发大于实现。

有朋友提醒我，996，NGO，区块链是几个毫不相干的圈子，之间并不互相理解，难以产生什么交集。但我觉得这不也正是开源世界的魅力，万一哪天有人觉得有趣，就实现了呢。



