repChain教程
=============

8.1 构建第一个repChain网络
-----------------------------

.. note::

	这些实例已在标记为sc1.17的发行版本中验证，如果你使用的是其他版本或分支，可能会产生一些错误导致实例不能正确运行。

8.1.1 安装预置环境
++++++++++++++++++++

虽然RepChain可运行于Windows平台和Linux平台，如果想更方便地观察实时出块、交易信息的可视化图形，或者方便开发，
建议你在Windows系统下搭建，生产环境推荐Linux平台。当然，这取决于您个人的开发习惯及公司政策。

**Linux下搭建RepChain环境**


	*安装JDK*

	切换到root用户下依次执行下列命令:
	
	.. code-block:: Java
		:linenos:
		
		// 添加Java源
		add-apt-repository ppa:webupd8team/java
		
		// 更新源
		apt-get update
		
		// 查看Java可用版本
		apt-cache show oracle-java
		
		// 选择安装JDK8
		apt-get install oracle-java8-installer

	如果安装不成功，可手动下载JDK安装。依次执行下列命令:
	
	.. code-block:: Java
		:linenos:

		// 下载OracleJDK
		wget http://download.oracle.com/otn-pub/java/jdk/8u161-b12/jdk-8u161-linux-x64.tar.gz
		
		// 解压文件
		tar -zxvf jdk-8u161-linux-x64.tar.gz
		
		// 移动文件到安装目录
		sudo mkdir /usr/lib/jdk    
		sudo mv ~/Downloads/ jdk1.8.0_161  /usr/lib/jdk/jdk1.8
	
	.. note::
	
		移动后jdk1.8.0_161被重命名为jdk1.8

	设置环境变量

	在/etc/profile文件的末尾添加下列环境变量后保存退出
	
	.. code-block:: Java
		:linenos:
		
		// 打开/etc/profile文件
		vim /etc/profile 
		
		// 在profile的末尾添加下列变量
		export JAVA_HOME=/usr/lib/jdk/jdk1.8
		export JRE_HOME=${JAVA_HOME}/jre
		export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
		export PATH=.:${JAVA_HOME}/bin:$PATH
	
	使配置立即生效::
	
		>source /etc/profile
	
	检测jdk是否安装成功

	执行下列命令，若JDK安装并正确配置，你将会看到JDK及JRE的版本::

		>java -version

	若切换到用户模式显示java没有安装，只需要在用户模式下输入以下命令后回车再次配置环境变量即可解决

	.. code-block:: Java
		:linenos:
		
		export JAVA_HOME=/usr/lib/jdk/jdk1.8
		export JRE_HOME=${JAVA_HOME}/jre
		export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
		export PATH=.:${JAVA_HOME}/bin:$PATH

	安装python2.7

		sudo apt-get install python2.7

	确保python安装成功::

		python2.7 --version

	1) 自动安装安装SBT

	.. note::
	
		可能会因为网络等原因导致安装文件不全等失败，因此建议采用手动下载安装包的方式安装。
		若你仍坚持使用该方式，请依次执行下列命令：
	
	.. code-block:: Java
		:linenos:
		
		// 添加SBT源
		echo "deb https://dl.bintray.com/sbt/debian /" | sudo tee -a /etc/apt/sources.list.d/sbt.list
		
		// 添加key
		sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 2EE0EA64E40A89B84B2DF73499E82A75642AC823
		
		// 更新软件
		sudo apt-get update
		
		// 安装SBT
		sudo apt-get install sbt

	安装完成后可通过which命令查看SBT的安装目录，如下图：
	
	.. image:: ./images/chapter8/whichsbt.png
	   :height: 110
	   :width: 1000
	   :scale: 100
	   :alt: sbt安装路径

	   
	2) 手动安装sbt

	在使用自动安装方式安装sbt，运行RepChain时sbt一直不能启动，所以采用手动安装配置环境变量。
	依次执行下列命令::

		// 下载SBT压缩包
		wget https://github.com/sbt/sbt/releases/download/v1.1.1/sbt-1.1.1.tgz
		
		// 建立目录/opt/scala/sbt，解压sbt到/opt/scala
		zxvf ~/Downloads/sbt-1.1.1.tgz -C /opt/scala/

	建立sbt启动脚本::

		cd /opt/scala/sbt/

		vim sbt

	添加下面的内容::

		SBT_OPTS="-Xms512M -Xmx1536M -Xss1M -XX:+CMSClassUnloadingEnabled -XX:MaxPermSize=256M"
		java $SBT_OPTS -jar /opt/scala/sbt/bin/sbt-launch.jar "$@" 

	修改sbt文件权限::

		chmod u+x sbt

	配置sbt环境变量::

		// 编辑bashrc
		vim ~/.bashrc
		
		// 在bashrc的末尾添加path变量后保存退出
		export PATH=/opt/scala/sbt/:$PATH


	使配置文件立即生效::

		source ~/.bashrc

	测试sbt是否安装成功::

		sbt sbt-version
		
	.. note::
	
		若sbt服务一直启动不了，请在root下重新配置环境变量
		

	安装scala环境

		Scala采用手动下载压缩包的方式安装。
	
	.. code-block:: Java
		:linenos:

		// 获取scala压缩包
		wget https://downloads.lightbend.com/scala/2.12.4/scala-2.12.4.tgz
		
		// 安装目录
		cd  /usr/local/share
		
		// 解压压缩包到安装目录
		tar -xzvf /home/vic/Downloads/scala-2.12.4.tgz -c /usr/local/share/scala

	配置环境变量（root环境）
	
	在/etc/profile中添加下面的变量::

		export SCALA_HOME=/usr/local/share/scala
		export PATH=${SCALA_HOME}/bin:$PATH

	使配置立即生效::

		source /etc/profile

	测试scala是否安装成功::
	
		scala -version
	
	.. image:: ./images/chapter8/scala_version.png
	   :height: 680
	   :width: 1260
	   :scale: 50
	   :alt: scala版本
	

**Windows系统搭建RepChain环境**

	Windows上搭建RepChain环境相对于Linux来说要简单许多，至少对于习惯在Windows上开发的人员来说是这样。
	
	**环境要求**

	* JDK1.8+
	* Scala插件
	* Idea IDE
	* Python2.7
	* Sbt插件

	网络上关于Windows上安装JDK及python的教程有很多，因此不再赘述。
	接下来的操作假设你已经完成以下步骤：

	1. JDK已经安装并正确配置环境变量
	2. Python已经安装并正确配置环境变量
	3. Idea IDE已经安装
	
	**安装scala插件**

	File -> Settings -> Plugins -> Install JetBrains Plugins，搜索scala，下载安装完成后重启idea。

	.. note::
	
		scala插件中已经包含了sbt插件
			
	从https://gitee.com/chen4w/repchain下载RepChain源码节后在idea中打开项目，根据自己使用的IDE在project/scalapb.sbt中选择使用sbt-idea或sbteclipse-plugin

	.. code-block:: Java
		:linenos:
		
		resolvers += "Sonatype snapshots" at "https://oss.sonatype.org/content/repositories/snapshots/"
		addSbtPlugin("com.github.mpeltonen" % "sbt-idea" % "1.7.0-SNAPSHOT")
		//addSbtPlugin("com.typesafe.sbteclipse" % "sbteclipse-plugin" % "5.1.0")
	
	请自行在ProjectStructure中配置SDK和scala SDK。完成后下载项目依赖包。

8.1.2.启动网络
+++++++++++++++++

	完成前面的所有步骤后，repchain的预置环境就已经具备了。现在让我们来运行RepChain吧。
	
	执行命令::

		// RepChain的源码下载需要git工具，若你还没有安装，请先安装git
		//检查git是否可用
		git --version
		
		//获取RepChain源码
		git clone https://gitee.com/chen4w/RepChain.git

	构建scalapb.sbt::

		cd /home/vic/repchain
		
		vim ./project/scalapb.sbt

	在scalapb.sbt中添加依赖
	
	.. code-block:: Java
		:linenos:

		addSbtPlugin("com.thesamet" % "sbt-protoc" % "0.99.12")
		libraryDependencies += "com.trueaccord.scalapb" %% "compilerplugin" % "0.6.6"
	
	构建build.sbt::

		vim build.sbt
		
	.. code-block:: Java
		:linenos:
		
		PB.targets in Compile := Seq(
		  scalapb.gen() -> (sourceManaged in Compile).value
		)

	启动RepChain服务

	.. code-block:: Java
		:linenos:
		
		cd repchain
		
		// 启动sbt
		sbt
		
		//编译repchain，主要是将protobuf定义编译为scala的类 
		compile
		
		//运行repchain
		run

	如果一切OK，repchain就已经在成功运行了，你将会在终端看到实时打印的日志
	
	.. image:: ./images/chapter8/repchain_console_log.png
	   :scale: 50
	   :height: 722
	   :width: 1341
	   :alt: repchain后台打印日志
	   
	在浏览器中访问localhost:8081/web/g1.html，你将会看到交易可视化图形，前端交易日志以及区块信息
	
	.. image:: ./images/chapter8/repchain_ui_view.png
	   :scale: 50
	   :height: 1188
	   :width: 1350
	   :alt: repchain交易可视化图形
	
	.. image:: ./images/chapter8/repchain_ui_log.png
	   :scale: 50
	   :height: 668
	   :width: 1335
	   :alt: repchain前端交易日志
   
   
	.. image:: ./images/chapter8/repchain_ui_block.png
	   :scale: 50
	   :height: 541
	   :width: 1295
	   :alt: repchain前端区块信息
   

8.1.3.关闭网络
++++++++++++++++++++

	RepChain关闭网络和退出应用一样简单，如果你是Linux下使用sbt运行，只需要ctrl+c停止服务即可。

8.1.4.链码部署及调用
+++++++++++++++++++++++++

	**安装和实例化链码**

	RepChain提供了部署链码的API接口/transaction/{codebody}，提交方式为post，请求格式是application/json。
			
	.. code-block:: json
		:linenos:
	
		{
		  "stype": 1,
		  "idPath": "",
		  "iptFunc": "",
		  "iptArgs": [],
		  "timeout": 0,
		  "secureContext": "string",
		  "code": "string",
		  "ctype":0
		}
		
	部署链码时通常只关注stype和code两个参数，其中stype代表交易的方式，值为1时表示部署链码，值为2时表示调用链码。
	
	code是部署的链码内容。现在来部署一个简单的链码：
	
	.. code-block:: json
		:linenos:
	
		{
		  "stype": 1,
		  "idPath": "",
		  "iptFunc": "",
		  "iptArgs": [],
		  "timeout": 0,
		  "secureContext": "string",
		  "code": "function write(pm){for(x in pm){shim.setVal(x,pm[x]);}}function read(pn){return shim.getVal(pn);}function transfer(afrom,ato,amount){var rfrom = read(afrom);if(rfrom<amount)throw '余额不足!';var rto = read(ato);write(afrom,rfrom-amount);write(ato,rto+amount);}",
		  "ctype":0
		}
		
	该链码包含三个函数，write()完成赋值，read()完成查询操作，transfer()则完成模拟的转账功能。

	部署完成后返回部署的结果：
	
	.. code-block:: json
		:linenos:

		{
		"txid": "b9307715189f11b04d2fc9152184b119ceb78a0ef9780e0e13aa44e6491b24f2",
		  "result": "b9307715189f11b04d2fc9152184b119ceb78a0ef9780e0e13aa44e6491b24f2",
		  "ol": [
			{
			  "key": "c_b9307715189f11b04d2fc9152184b119ceb78a0ef9780e0e13aa44e6491b24f2",
			  "oldValue": null,
			  "newValue": []
			}
		  ]
		}
		
	至此，一个简单的链码便部署完成。
	result：该链码的id，是chaincode的唯一标识，调用链码时必须指定链码id

	**调用链码**

		在上一小节中我们部署了一个具有三个功能的简单链码，现在通过该链码来演示链码的调用。

		首先调用write()函数为老板vic（呃...实际上vic是个穷x职员）和员工Jenna分别存入1000000元和0元：
		
	.. code-block:: json
		:linenos:

		{
		  "stype": 2,
		  "idPath": "",
		  "idName": "b9307715189f11b04d2fc9152184b119ceb78a0ef9780e0e13aa44e6491b24f2",
		  "iptFunc": "write({'vic':1000000},{'Jenna':100});",
		  "iptArgs": [],
		  "timeout": 0,
		  "secureContext": "string",
		  "code": "string",
		  "ctype":0
		}
	
	假设Jenna月薪4000元，现在是时候发工资了，我们将调用transfer()来给Jenna发工资：
	
	.. code-block:: json
		:linenos:

		{
		  "stype": 2,
		  "idPath": "",
		  "idName": "b9307715189f11b04d2fc9152184b119ceb78a0ef9780e0e13aa44e6491b24f2",
		  "iptFunc": "transfer('vic','Jenna',4000);",
		  "iptArgs": [],
		  "timeout": 0,
		  "secureContext": "string",
		  "code": "string",
		  "ctype":0
		}

	**查询**

	为确保链码被正确部署，levelDB被填充，我们查询一下Jenna的账户余额，看这个月的工资是否到账：
	
	.. code-block:: json
		:linenos:

		{
		  "stype": 2,
		  "idPath": "",
		  "idName": "b9307715189f11b04d2fc9152184b119ceb78a0ef9780e0e13aa44e6491b24f2",
		  "iptFunc": "read(‘Jenna’);",
		  "iptArgs": [],
		  "timeout": 0,
		  "secureContext": "string",
		  "code": "string",
		  "ctype":0
		}
	
	查询结果：

	.. image:: ./images/chapter8/queryResult.png
	   :scale: 50
	   :height: 608
	   :width: 1033
	   :alt: 查询余额

	可以看到查询结构与预期相符，证明chaincode已经被成功部署。

	幕后发生了什么？

	RepChain的交易其实并非像看上去这么简单，哪怕是一个简单的查询操作。与传统的应用不同，RepChain并不是向数据库请求数据返回结果就行，背后还有一系列的事情要做。
	首先，交易的发起人提交交易申请，系统接收交易请求后构建transaction并通过peer节点广播；预执行交易成功则向全网成员广播交易。获取签名证书，成员投票达成共识，背书节点背书并全网广播，交易成功，区块同步，持久化数据。

	这指明了什么？

	为了能够正确地在账本上进行读写操作，链码必须被安装在peer节点上。此外，每个peer节点的链码服务的容器除了deploy或者传统的交易-读/写-针对该链码服务执行（例如查询vic的值），在其他情况下不会启动。当然，所有信道中的节点都持有以块的形式顺序存储的不可变的账本精确的备份，以及状态数据库来保存前状态的快照。这包括了没有在其上安装链码服务的peer节点。最后，链码在被安装后将是可达状态，因为它已经被实例化了。

	我如何查询这些交易？

	RepChain的发生的每笔交易都是可追溯且不可修改的，这也是区块链“不可抵赖性”的体现。每笔交易都会有一个唯一标识的id，通过此id即可查询到交易的详细内容。交易的查询接口为localhost:8081/transaction/{transactionId}。

	为了演示交易的查询，我们先创建一笔交易，比如查询vic的余额。
	
	.. code-block:: json
		:linenos:

		{
		  "stype": 2,
		  "idPath": "",
		  "idName": "b9307715189f11b04d2fc9152184b119ceb78a0ef9780e0e13aa44e6491b24f2",
		  "iptFunc": "read('vic');",
		  "iptArgs": [],
		  "timeout": 0,
		  "secureContext": "string",
		  "code": "string",
		  "ctype":0
		}
	
	交易的结果是这样的：
	
	.. code-block:: json
		:linenos:

		{
		  "txid": "f7c34dc0-2b2b-11e8-a39b-0f000027000a",
		  "result": 1000000,
		  "ol": []
		}
	
	现在通过txid来查看这笔交易的内容：Localhost:8081/transaction/f7c34dc0-2b2b-11e8-a39b-0f000027000a

	从下面的查询结果可以清楚的看到该交易的类型、chaincodeId、执行的函数、交易时间、证书及签名等详细信息：
	
	.. code-block:: json
		:linenos:

		{
		  "result": {
			"type": "CHAINCODE_INVOKE",
			"chaincodeID": "cGF0aDogIiIKbmFtZTogImI5MzA3NzE1MTg5ZjExYjA0ZDJmYzkxNTIxODRiMTE5Y2ViNzhhMGVmOTc4MGUwZTEzYWE0NGU2NDkxYjI0ZjIiCg==",
			"payload": {
			  "chaincodeID": {
				"name": "b9307715189f11b04d2fc9152184b119ceb78a0ef9780e0e13aa44e6491b24f2"
			  },
			  "ctorMsg": {
				"function": "read('vic');"
			  },
			  "timeout": 1000,
			  "secureContext": "secureContext",
			  "codePackage": "c3RyaW5n"
			},
			"metadata": "rO0ABXNyACBzY2FsYS5jb2xsZWN0aW9uLm11dGFibGUuSGFzaE1hcAAAAAAAAAABAwAAeHB3DQAAAu4AAAAAAAAABAB4",
			"txid": "f7c34dc0-2b2b-11e8-a39b-0f000027000a",
			"timestamp": "2018-03-19T12:14:07.516Z",
			"confidentialityProtocolVersion": "confidentialityProtocolVersion-1.0",
			"nonce": "bm9uY2U=",
			"toValidators": "dG9WYWxpZGF0b3Jz",
			"cert": "MU1IOXhlZFBUa1dUaEpVZ1Q4WlllaGlHQ003YkVaVFZHTg==",
			"signature": "MEUCIBWDXzYSA1wzSLQSG0TAZzoA7gwfYzXXgwt0j9caP6QkAiEA7nRgP6NAAk84ty54qdPF/KUuyBIio21+D4uhVmey4Mg="
		  }
		}
	
	我如何查询链码日志？

	目前RepChain还不支持查询链码日志，在后续的版本中会加入链码日志查询。不过在Users目录下的区块文件blockdata中也能看到调用的链码。

8.1.5.了解 Docker Compose
+++++++++++++++++++++++++++++

	Docker Compose 是 Docker 官方编排（Orchestration） 项目之一，负责快速在集群中部署分布式应用。
	Compose 定位是 「定义和运行多个 Docker 容器的应用（Defining and running multicontainer Docker applications） 」，
	其前身是开源项目 Fig。通过第一部分中的介绍，我们知道使用一个 Dockerfile 模板文件，可以让用户很方便的定义一个单独的应用容器。
	然而，在日常工作中，经常会碰到需要多个容器相互配合来完成某项任务的情况。例如要实现一个 Web 项目，
	除了 Web 服务容器本身，往往还需要再加上后端的数据库服务容器，甚至还包括负载均衡容器等。
	Compose 恰好满足了这样的需求。它允许用户通过一个单独的 docker-compose.yml 模板文件（YAML 格式） 来定义一组相关联的应用容器为一个项目（project） 。

	Compose 中有两个重要的概念：
	服务 ( service )：一个应用的容器，实际上可以包括若干运行相同镜像的容器实例。
	项目 ( project )：由一组关联的应用容器组成的一个完整业务单元，在 dockercompose.yml 文件中定义。
	Compose 的默认管理对象是项目，通过子命令对项目中的一组容器进行便捷地生命周期管理。Compose 项目由 Python 编写，实现上调用了 Docker 服务提供的 API 来对容器进行管理。因此，只要所操作的平台支持 Docker API，就可以在其上利用 Compose 来进行编排管理。 

8.1.6.使用LevelDB
+++++++++++++++++++

	与传统的关系型数据库相比，LevelDB存储数据的方式有所不同，它是通过键-值对（key-value）的方式存储数据，这与RepChain存储的数据结构有关。
	从前面的链码的部署和调用可以看出交易的数据库类型为json，数据结构正是key-value的结构，交易的内容最终要持久化到数据库存储，LevelDB无疑是非常合适的。
	当然除了LevelDB之外，还可以采用CouchDB来做数据持久化。

8.1.7.关于数据持久化的提示
+++++++++++++++++++++++++++++

	RepChain暂时以文件结构的方式做持久化，即在/User/username/目录下生成leveldb和blockdata，用于存储区块信息及交易数据，
	只是为了方便调试，在正式版本中我们会采用LevelDB做持久化，相关的细节届时会在RepChain开源社区上发布。
	建议你关注我们的开源社区，以便及时获取相关帮助信息。

8.1.8.故障排除
+++++++++++++++++

	故障内容及故障原因可能会因版本而异，不同的运行平台也会出现不同的异常，这里列举一些可能出现的交易相关的通用故障排除方法。

* 调用链码失败
	可能是调用的链码未在网络中部署，或链码的ID有误
	
* Bad Request异常
	请求的数据格式、参数类型与系统要求不符

8.2.Chaincode指南
---------------------

	简单来说Chaincode是一段能够实现预定义接口的程序，它通过应用提交的交易来对账本进行初始化操作和管理。
	Repchain的chaincode由scala语言编写，同时支持其他语言（如nodejs，Java等）。Chaincode运行在一个受保护的Docker容器当中，
	与背书节点的运行互相隔离。

	由于chaincode通常处理由网络中的成员一致认可的业务逻辑，因此chaincode也被称为“智能合约”。每段chaincode创建的账本状态彼此之间是相互独立的，
	如chaincodeA不能直接访问chaincodeB创建的账本状态，但是在相同的网络中，一段chiancode在获取相应许可后则可以调用其他chiancode来访问它的账本。
	关于chaincode的详细说明请参阅针对chaincode应用开发的《Chaincode 开发手册》部分，如果你只是想通过repchain来安装、实例化与升级chaincode而不涉及具体开发，
	则可以参阅《Chaincode 操作手册》部分。

8.3.Chaincode开发手册
------------------------

8.3.1.什么是Chaincode？
++++++++++++++++++++++++++

	简单来说Chaincode是一段能够实现预定义接口的程序，它通过应用提交的交易来对账本进行初始化操作和管理。
	Repchain的chaincode由scala语言编写，同时支持其他语言（如nodejs，Java等）。Chaincode运行在一个受保护的Docker容器当中，
	与背书节点的运行互相隔离。
	由于chaincode通常处理由网络中的成员一致认可的业务逻辑，因此chaincode也被称为“智能合约”。
	每段chaincode创建的账本状态彼此之间是相互独立的，如chaincodeA不能直接访问chaincodeB创建的账本状态，
	但是在相同的网络中，一段chiancode在获取相应许可后则可以调用其他chiancode来访问它的账本。
	在接下来的章节中，我们会以一个应用开发者的视角了解学习chaincode。我们将通过展示一个chaincode的应用范例来搞清每一个Chaincode Shim API的来龙去脉。

8.3.2.Chaincode API
++++++++++++++++++++++++
    
	每个chaincode程序都必须实现 chiancode接口 ，接口中的方法会在响应传来的交易时被调用。Deploy方法用于chaincode的部署，
	Invoke（调用）方法会在响应invoke（调用）交易时被调用以执行交易。
	其他chaincode shim APIs中的接口被称为chaincode存根接口，用于访问及修改账本，并实现chaincode之间的互相调用。
	例如shim.getVal()用于从账本中获取数据，shim.setValue()用于更新账本的数据，loadCert()则实现本地加载证书到信任证书列表，从而通过交易完成入网许可。
	在本篇指南中，我们会通过实现一个能管理简单“资产”的chaincode应用范例来演示这些接口的使用方法。
	如果您之前还未编写过scala程序，那么您也许需要先确保 scala程序设计语言 在您的系统中已经被正确配置。
	直接在项目的..\src\main\scala路径下创建源文件ContractAssetsTPL.scala


该合约的内容如下所示，该合约功能很简单，set()方法完成资产的更新，transfer()则完成转账功能。

	.. code-block:: java
	   :linenos:
   
		/**
		 * 资产管理合约
		 */
		class ContractAssetsTPL extends Contract{
		case class Transfer(from:String, to:String, amount:Int)

		  implicit val formats = DefaultFormats
			def init(ctx: ContractContext){      
			}
			
			def set(ctx: ContractContext, data:Map[String,Int]):Object={
			  for((k,v)<-data){
				ctx.api.setVal(k, v)
			  }
			  null
			}
			
			def transfer(ctx: ContractContext, data:Transfer):Object={
			  val sfrom =  ctx.api.getVal(data.from)
			  var dfrom =sfrom.toString.toInt
			  if(dfrom < data.amount)
				throw new Exception("余额不足")
			  var dto = ctx.api.getVal(data.to).toString.toInt
			  ctx.api.setVal(data.from,dfrom - data.amount)
			  ctx.api.setVal(data.to,dto + data.amount)
			}
			/**
			 * 根据action,找到对应的method，并将传入的json字符串parse为method需要的传入参数
			 */
			def onAction(ctx: ContractContext,action:String, sdata:String ):Object={
			  val json = parse(sdata)
			  action match {
				case "transfer" => 
				  transfer(ctx,json.extract[Transfer])
				case "set" => 
				  set(ctx, json.extract[Map[String,Int]])
			  }
			}
		  }
  
8.3.3.实现Chaincode应用
++++++++++++++++++++++++++

	如上文所述，我们的chaincode应用实现了两个函数，并可以被Invoke函数调用。下面我们就来真正实现这些函数。
	注意，就像上文一样，我们调用chaincode shim API中的Shim.setState、Shim.getState及Shim.get函数来访问账本。
	这里给出了这三个函数的实现：
	
	.. code-block:: Java
	   :linenos:
	   
		def setState(key: Key, value: Array[Byte]): Unit = {
		  val pkey = pre_key + key
		  val oldValue = get(pkey)
		  sr.Put(pkey, value)
		  //记录初始值
		  if (!mb.contains(pkey)) {
			mb.put(pkey, oldValue)
		  }
		  //记录操作日志
		  ol += new Oper(key, oldValue, value)
		}

		// Get returns the value of the specified asset key
		private def get(key: Key): Array[Byte] = {
		  sr.Get(key)
		}

		def getState(key: Key): Array[Byte] = {
		  get(pre_key + key)
		}

	编译Chaincode

	如果没有报错，那么我们就可以进行下一步：测试chaincode。实际上编译通过后我们什么都不需要做，系统启动时会自动初始化链码完成交易，前提是在system.conf配置文件中将交易生产方式trans_create_type的值更改为1，即自动交易，之后会在控制台的打印日志中看到合约被执行。另外也可以通过实时的交易图形观察到交易以及区块数量的增加。

8.4.Chaincode操作手册
--------------------------

8.4.1.什么是Chaincode？
	Chaincode是一段由scala语言编写（支持其他编程语言，如nodejs），并能实现预定义接口的程序。一段chaincode通常处理由网络中的成员一致认可的业务逻辑，故我们很可能用“智能合约”来代指chaincode。一段chiancode创建的（账本）状态是与其他chaincode互相隔离的，故而不能被其他chaincode直接访问。每段Chaincode拥有自己的chaincodeId，调用时必须指定chaincodeId。
