RepChain操作指南
===========================

10.1 安装
-------------

	* git clone https://gitee.com/chen4w/RepChain
	* install jdk8+
	* install python
	* install Scala
	* install SBT
	* install scala IDE
	* install keystore-explorer ——用于生成密钥对的工具,非必须
	* install protobuf editor——编辑protobuf定义工具，非必须

10.2 配置
----------
10.2.1 硬件配置
++++++++++++++++++

	.. image:: ./images/chapter10/hardwareconf.png
	   :height: 442
	   :width: 1464
	   :scale: 50
	   :alt: 软件配置要求

	上面描述的硬件配置为最低配置，在满足最低配置的要求下，尽量提高配置。

10.2.2 软件配置
+++++++++++++++++

	.. image:: ./images/chapter10/softwareconf.png
	   :height: 898
	   :width: 1464
	   :scale: 50
	   :alt: 软件配置要求

10.3. 系统配置和相关文件
------------------------
10.3.1 程序目录结构
++++++++++++++++++++++

	.. image:: ./images/chapter10/mulujiegou.png
	   :height: 1907
	   :width: 1394
	   :scale: 50
	   :alt: 程序目录结构
   
10.3.2 系统配置说明
++++++++++++++++++++++

	.. image:: ./images/chapter10/sysconf.png
	   :height: 919
	   :width: 1465
	   :scale: 50
	   :alt: 系统配置说明
   
10.3.3 存储配置说明
+++++++++++++++++++++

	.. image:: ./images/chapter10/storeconf.png
	   :height: 316
	   :width: 1464
	   :scale: 50
	   :alt: 系统配置说明

10.3.4 日志配置说明
+++++++++++++++++++++++

	针对logback日志框架的配置文件。详情见官网：https://logback.qos.ch/manual/configuration.html

10.4 系统部署
------------------

10.4.1 Linux
++++++++++++++++

	1)git clone https://gitee.com/chen4w/RepChain.git

	.. image:: ./images/chapter10/gitclone.png
	   :height: 250
	   :width: 1464
	   :scale: 50
	   :alt: 获取RepChain源码
   
	2)进入RepChain目录（cd RepChain），并执行sbt（sbt）

	.. image:: ./images/chapter10/enterRepChain.png
	   :height: 216
	   :width: 1465
	   :scale: 50
	   :alt: 进入RepChain目录

	3)执行compile，根据build.sbt下载依赖包并编译（compile）

	.. image:: ./images/chapter10/compile.png
	   :height: 254
	   :width: 1465
	   :scale: 50
	   :alt: 编译RepChain
	   
	.. image:: ./images/chapter10/compiledone.png
	   :height: 46
	   :width: 1465
	   :scale: 50
	   :alt: 编译RepChainc成功


	4)修改build.sbt中mainClass in (Compile, packageBin) := Some("rep.app.RepChain")的类名，并执行assembly命令打包（assembly）

	.. image:: ./images/chapter10/assembly.png
	   :height: 1217
	   :width: 1465
	   :scale: 50
	   :alt: 打包RepChain

	.. image:: ./images/chapter10/assembly2.png
	   :height: 926
	   :width: 1465
	   :scale: 50
	   :alt: 打包RepChain
	   
	从截图中可以看到打包命令执行成功，jar包输出在RepChain/target/scala-2.11/RepChain.jar

	5)把jar包，相关的配置文件放到同一个目录下

	.. image:: ./images/chapter10/packRepChain.png
	   :height: 93
	   :width: 1465
	   :scale: 50
	   :alt: 集成配置文件

	6)当前目录下执行命令:java -Dlogback.configurationFile=conf/logback.xml -jar RepChain.jar

	.. image:: ./images/chapter10/executeRepChain.png
	   :height: 480
	   :width: 1465
	   :scale: 50
	   :alt: 运行jar包
	   
	7)浏览器输入http://localhost:8081/web/g1.html

	.. image:: ./images/chapter10/RepChain_view.png
	   :height: 716
	   :width: 1465
	   :scale: 50
	   :alt: RepChain可视化界面

	8)浏览器输入http://localhost:8081/swagger/index.html

	.. image:: ./images/chapter10/swaggerui.png
	   :height: 843
	   :width: 1465
	   :scale: 50
	   :alt: swaggerui

10.4.2 Windows
+++++++++++++++++++++

	1. git clone download the project to local。下载源码到本地
	2. under the project root path,sbt to download dependencies.(maven默认仓库下载龟速,应使用阿里镜像)。下载依赖包，解决依赖关系
	3. compile to generate protobuf scala class. 执行compile命令编译
	4. eclipse to generate eclipse project settings.
	5. open scala IDE, File->Import->Existing Projects into Workspace。导入编辑器
	6. right click rep.app.RepChain.scala,Run As->Scala Application(单机组网5个节点)
	7. Run configuration 配置VM参数 -Dlogback.configurationFile=conf/logback.xml (使logback配置生效)
	8. view realtime graph http://localhost:8081/web/g1.html
	9. view rest apis http://localhost:8081/swagger/index.html

10.5 修改配置
-------------------

10.5.1 Keytool工具使用
+++++++++++++++++++++++++

	RepChain中目前预置了密钥对和证书，如果用户想自己生成，可以采用keytool工具生成密钥对，导出证书，并形成证书信任列表，下面在Winsows下详细进行说明。

	**(1) keytool简介**
	
	keytool 是jdk自带的密钥和证书管理工具，它使用户能够管理自己的公钥、私钥对及相关证书，用于通过数字签名自我认证。在JDK 1.4以后的版本中都包含了这一工具，它的位置为%JAVA_HOME%\bin\keytool.exe，如下图所示是keytool工具。
	
	.. image:: ./images/chapter10/keytools.png
	   :height: 635
	   :width: 1238
	   :scale: 50
	   :alt: keytools
	
	.
	
	**keytool用法：**
	
	.. image:: ./images/chapter10/HowToUseKeytool.png
	   :height: 825
	   :width: 1178
	   :scale: 50
	   :alt: keytools使用方法

	.
	   
	**(2) 创建证书：**
	
	创建证书主要是使用" -genkeypair"，该命令的可用参数如下：
	
	.. image:: ./images/chapter10/CreateKeystore.png
	   :height: 843
	   :width: 1204
	   :scale: 50
	   :alt: 创建证书
	
	.
	
	这里给出一个范例：生成一个test证书：
	
	.. code-block:: shell
	   :linenos:
	   
	   keytool -genkeypair -alias "test" -keyalg "RSA" -keystore "test.keystore"
	
	**功能：**
	
	创建一个别名为test的证书，该证书存放在名为test.keystore的密钥库中，若test.keystore密钥库不存在则创建。
	
	参数说明：
	
	* genkeypair：生成一对非对称密钥；
	* alias：指定密钥对的别名，该别名是公开的；
	* keyalg：指定加密算法，本例中的采用通用的RAS加密算法；
	* keystore：密钥库的路径及名称，不指定的话，默认在操作系统的用户目录下生成一个".keystore"的文件。
	
	再输入命令之后，如下图所示，必选项是密码和域名，其他可以不填。
	
	**注意：**
	
	1. 密钥库的密码至少必须六个字符，包含字母和数字。
	2. 名字与姓氏应该是输入的域名。
	
	.. image:: ./images/chapter10/CreateKeystoreExp.png
	   :height: 763
	   :width: 1090
	   :scale: 50
	   :alt: 创建证书示例
	
	.
	
	执行完上述命令后，在操作系统的用户目录下生成了一个"test.keystore"的文件，如下图所示：
	
	.. image:: ./images/chapter10/test_keystore.png
	   :height: 497
	   :width: 1219
	   :scale: 50
	   :alt: 已创建的test证书
	
	.
	
	查看密钥库的证书：这里给出一个范例，查看test.keystore这个密码库中所有的证书。
	
	.. code-block:: shell
	   :linenos:
	   
	   keytool -list -keystore test.keystore
	
	.. image:: ./images/chapter10/ViewKeystore.png
	   :height: 754
	   :width: 1077
	   :scale: 50
	   :alt: 查看密钥库证书
	   
	.
	
	**(3) 导出证书：**
	
	范例：将名为test.keystore的证书库中别名为test的证书条目导出到证书文件test.crt或test.cer中：
	
	.. code-block:: shell
	   :linenos:
	   
	   keytool -export -alias test -file test.crt -keystore test.keystore
	   keytool -export -alias test -file test.cer -keystore test.keystore
		
	.. image:: ./images/chapter10/ExportKeystore.png
	   :height: 747
	   :width: 1067
	   :scale: 50
	   :alt: 导出证书
	
	.
	
	运行结果：在操作系统的用户目录下生成了一个"test.crt"和"test.cer"的文件，如下图所示：
	
	.. image:: ./images/chapter10/ExportKeystoreResult.png
	   :height: 643
	   :width: 1364
	   :scale: 50
	   :alt: 导出证书结果
	
	.
	
	**(4) 导入证书：**
		
	范例：将证书文件test.cer导入到名为test_cacerts的证书库中：
	
	.. code-block:: shell
	   :linenos:
	   
	   keytool -import -keystore test_cacerts -file test.cer
	
	.. image:: ./images/chapter10/ImportKeystore.png
	   :height: 815
	   :width: 1126
	   :scale: 50
	   :alt: 导入证书
	  
	.
	
	用户可以通过以上操作，生成密钥和证书，作为RepChain在身份认证过程中需要的证明，在形成信任列表之后即可使用。
	
10.5.2 最低配置
++++++++++++++++

	在此给出最低配置的说明：
	
	.. image:: ./images/chapter10/RowestConf.png
	   :height: 461
	   :width: 1464
	   :scale: 50
	   :alt: 最低配置
	   
5.3创世块制作
+++++++++++++++++++++

	源码内rep.utils.GensisBuilder组件用于生成创世块json文件，该json文件可以在链初始化时由节点加载，统一网络内节点加载的创世块必须相同。
	在创世块中的交易发起人为该节点的超级管理员，预置资产管理合约。创世块中预置deploy基础方法的交易，deploy用来部署合约，创世块中的交易发起人是超级管理员，预置资产管理合约，
	然后创世块出块：Gensis Block，并载入账本。
	
10.6 系统测试
-----------------

	* 单机多节点测试
	* 多机多节点测试

10.7 系统运行
---------------

本系统可以分为2种方式部署：

	1. 单机多节点部署：在一台机器启动一个实例，该实例中包含多个区块链对等节点。
	2. 多机多节点部署：在一台或者多台机器上启动多个实例，每个实例就是一个区块链对等节点。

	系统在运行时，IDE环境中需要配置VM参数 -Dlogback.configurationFile=conf/logback.xml (使logback配置生效)。运行jar包时需要指定参数java -Dlogback.configurationFile= conf/logback.xml-jar RepChain.jar

10.7.1单机多节点部署
++++++++++++++++++++++

	单机多节点部署运行的Main类是:rep.aap.RepChain.scala 可以在文件里面设置运行节点的个数默认（4/5）

	Conf/Store.properties设置存储路径，一般默认
	
	Conf/system.conf 设置系统运行的参数主要的有下面几项：
	
		* System.ws_enable 设置是否开启浏览器、API接口
		* System.trans_create_type 设置是否开启自动交易
	
	其余的默认即可，如有需要自行修改。
	
	最后可以在IDE中运行RepChain或者运行已经打好的jar包。

10.7.2 多机多节点部署
++++++++++++++++++++++++

	多机多节点部署运行的Main类是:rep.aap.RepChain_Single.scala

	Conf/Store.properties设置存储路径，一般默认
	
	Conf/system.conf 设置系统运行的参数主要的有下面几项：
	
	* System.ws_enable 设置是否开启浏览器、API接口
	* System.trans_create_type 设置是否开启自动交易
	* Akka.cluster.seed-nodes 种子节点列表，列表中的第一个会默认为leader,节点启动后会依照顺序发出加入请求，所以在这里一定要注意确保leader节点启动之后再启动其他节点，不然集群一直处于不收敛状态。在这里最好设置一个稳定的种子节点作为leader，
	* Akka.remote.netty.ssl 设置节点的ip地址和端口，如果使用了NAT地址映射需要指定内部地址和外部地址。

	.. image:: ./images/chapter10/multiconf.png
	   :height: 537
	   :width: 1220
	   :scale: 50
	   :alt: 多机组网配置

	其余的默认即可，如有需要自行修改。最后可以在IDE中运行RepChain或者运行已经打好的jar包。

	另：单机节点在运行时需要在后面加参数来区分节点，如java -Dlogback.configurationFile=conf/logback.xml -jar RepChain.jar 1。1用来唯一标志该节点
	再次强调节点必须一个一个运行，一个启动后再启动另一个，leader节点最先启动。
