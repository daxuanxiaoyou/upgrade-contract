# upgrade-contract
a upgrade contract demo based on openzeppelin

**初始部署**
  **first:先部署逻辑合约**
	  logic address: 0xd9145CCE52D386f254917e481eB44e9943F39138
  **second:再部署管理合约，用于升级逻辑合约**
	  Proxy admin address: 0xd8b934580fcE35a11B58C6D73aDeE468a2833fa8
  **third:部署可升级代理合约，用于存储逻辑合约数据**
	  部署时，需要指定三个参数：
		1. logic address
		2. Proxy admin address
		3. data:逻辑合约的初始化方法encode:abi.encodeWithSignature("initialize()")
	  proxy address: 0xf8e81D47203A594245E36C48e151709F0C19fBe8*

**至此，初始部署完成**
  如何检查代理合约地址是否有效
    1. remix选中[逻辑合约]的代码，然后在[at address]填入代理合约地址，生成逻辑合约。本例中填入 0xf8e81D47203A594245E36C48e151709F0C19fBe8
    2. 根据生成的合约，进行合约方法的调用，本例中调用set方法；然后查看get方法是否成效。
    3. 然后可以查看部署的逻辑合约，查看一下get方法，是否能查到上一步的set（不会查到，因为数据存在了代理合约的内存空间）

**升级逻辑合约，需要按照以下步骤**
1. 修改逻辑合约代码，然后部署新的逻辑合约
	0x7EF2e0048f5bAeDe046f6BF797943daF4ED8CB47
2. 替换旧的逻辑合约
此时调用部署好的管理合约进行升级，此合约提供了两个升级方法

upgrade，需要传入proxy地址，新的逻辑实现地址。
upgradeAndCall，需要传入roxy地址，新的逻辑实现地址，初始化调用数据。
由于数据是保存在代理合约中，这份数据已经初始化过了，不需要再初始化，所以调用upgrade方法即可，传入参数如下：

	代理合约地址：0xf8e81D47203A594245E36C48e151709F0C19fBe8
	新的逻辑合约地址：0x7EF2e0048f5bAeDe046f6BF797943daF4ED8CB47

3. 用相同的方法测试，升级后的合约是否生效
之前set的value，返回值是否生效为 + 1；如果生效，那么升级成功；如果没有生效，升级失败

https://learnblockchain.cn/article/2920
