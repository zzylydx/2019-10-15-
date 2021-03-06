# 2019-10-15-
<h1>学习《精通比特币》</h1>

最近在看《精通比特币》这本书，虽然是中文版，但是我依然看的很慢，加上课程比较忙，有很多其他的可也需要完成，所以看的非常慢！！！！

在这期间有一些小问题吧：

1、客户在消费的时候，比如说Alice付给Bob 0.0150比特币，Alice支付了0.1比特币，但是仅仅找回了0.0845~，而不是0.085~，
其中有0.0005比特币给了旷工？是旷工的报酬？

“这个差值会就被矿工当作交易费，放到区块的交易里， 最终放进区块链帐薄中。”-----------书上的原话

2、“每个矿工会在他的区块中包含⼀个特殊的交易， 将新生成的比特币（当前每区块为25比特币） 作为报酬付到他自己的比特币地址”  
通过这句话看出，Alice的0.0005比特币好像不包括在旷工的报酬里

3、矿工挖矿也就是工作量证明，是指用SHA256加密算法不断地对区块头和一个随机数字进行哈希计算，
直到出现一个和预设值相匹配的解（到底找的什么东西，希望后面的学习能让我清楚！！！）

知识点：基于某个区块每产生新的区块，对这个交易来说就增加了一次“证明”，当区块一个个堆上来时，
这个交易变得指数级的越来越难被推翻 ，因此在网络中也获得越来越多的信任！！！（这段话还是不错的，敲一遍！）<br/>

“对于中本聪比特币原论文有关交易这一块的理解”

!["tran"](https://github.com/zzylydx/2019-10-15-/blob/master/image/transaction.png);（完全理解偏了！！！）

<h3>“最近无意中看到了一篇关于比特币交易中的签名与验证的博客，不得不说博主看的通透，也一并解决了我心中的的疑惑~”</h3>

***比特币的白皮书中transaction的部分，说的很清楚也都能看懂，但是他的这幅图我是一直有疑惑的***
!["tran"](https://upload-images.jianshu.io/upload_images/1260884-d487ee2a8b981801.png);

论文课上老师提出的为什么owner1不直接把钱转给owner3，而非要通过owner2呢？这个地方我至今依然存有疑惑//

其次我的另外一个问题是：在《精通比特币》中有关交易这一部分，有一个P2PKH的交易模型，比特币网络中大多数交易都是通过这种模型实现的，有锁定脚本和解锁脚本，但是他的具体实现过程我把它理解成，用户1向用户2转账，用户1用自己的私钥进行签名，并将其和自己对方的公钥打包发送给用户2，这也就是解锁脚本<sig><PubK(B)>（我还一直纳闷怎么是解锁脚本），用户2有锁定脚本OP_DUP OP_HASH160 <PubKHash(B)> OP_EQUALVERIFY OP_CHECKSIG，具体的其他过程我就不细说了，主要讲签名过程，我理解的是用户2拿用户1的公钥与用户1的sig进行验证，通过CHECKSIG，验证为true则交易完成，但其实仔细分析，用户2是将用户1发送过来的sig和pubk（B）进行了比较，所以很早就埋下了疑问的种子，我还请教过师兄，他因为有些遗忘了，所以也说不清楚，直到我看完这篇博客，我才恍然大悟！！！
博客地址：https://www.jianshu.com/p/a21b7d72532f
!["tran"](https://upload-images.jianshu.io/upload_images/1260884-e4217838ce43d52a.png);
  
看到这张图片，我的思路也渐渐明朗了，注意观察里面的橙色线条，先简单说一下里面的内容：
  
Input是要说明我打算花的这UTXO是从哪儿来的。-----------------解锁脚本

具体的参数包括：

txid : 引用的UTXO所在的那笔交易ID

vout : 引用的UTXO所在交易的输出中的序号（从0开始）

scriptSig : 解锁脚本，包含付款人对本次交易的签名(<sig>)和付款人公钥(<PubK(A)>)。

Output是要说明我打算生成几个UTXO，分别给谁，每个UTXO里面有多少BTC。-----------------锁定脚本

具体的参数包括：

value : 比特币数量

n : UTXO序号（从0开始）

scriptPubkey : 锁定脚本，包含命令（OP_DUP等）和收款人的公钥哈希（<PubKHash(B)>)。

  我们下面具体分析一下，看userB给userC发送btc，用到的是userA发送给他的那笔UTXO（不展开讲了，详情请看UTXO.md），所定脚本：pubkhash（B），解锁脚本：sig（B），pubk（B），它是通过解锁脚本去解锁之前的UTXO，也就是之前交易的输出，方式：通过用私钥进行签名，从而验证是这笔UTXO的拥有者，进而将余额放到下一个交易输出中，这时我们再回顾一下P2PKH的交易模型，最后一步，验证解锁脚本里面的sig和pubk是否匹配就说的通了，因为都是userB的，至此完成交易签名。
  
有关签名具体是怎样验证的，有空再码，先放张图~~~~

!["tran"](https://github.com/zzylydx/2019-10-15-/blob/master/image/%E6%A4%AD%E5%9C%86%E6%9B%B2%E7%BA%BF%EF%BC%88A%2BB%3DC%EF%BC%89.gif);
