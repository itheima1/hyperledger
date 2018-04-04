# 手把手教你hyperledger fabirc v1.1 

> 网上大多数hyperledger fabric的教程都是基于0.6或者1.0等比较老的版本, 主要采用go语言开发chaincode, 采用java-sdk去调用链码.

>从fabirc1.1开始,官方推荐使用nodejs去开发链码,node-sdk调用代码. 
传智播客物联网+区块链学院带您使用nodejs开发hyperledger.


## 0.环境搭建准备工作
建议使用ubuntu服务器,这里我直接使用了阿里云的乞丐版服务器,配置如下:

![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/421F84B5F00E4D1AADD0DDA109FEDA9D/136)


操作系统为Ubuntu 14.04(64位)为保证后续步骤一致,请使用跟我相同的版本.



## 1. 远超登录终端准备
用putty或者xshell远超连接进去

![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/F34684F24A1541C49EDAEE8E98D5D5A9/147)



## 2. 安装git
>apt-get update

>apt-get install git

![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/B81421F12EF749859012A426ED493736/159)


## 3. 安装docker-ce
请不要直接apt安装旧版本的docker

    [阿里云安装docker教程](https://yq.aliyun.com/articles/110806?spm=5176.8351553.0.0.5d4e1991URD8Ia)

```
# step 1: 安装必要的一些系统工具
sudo apt-get update
sudo apt-get -y install apt-transport-https ca-certificates curl software-properties-common
# step 2: 安装GPG证书
curl -fsSL http://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo apt-key add -
# Step 3: 写入软件源信息
sudo add-apt-repository "deb [arch=amd64] http://mirrors.aliyun.com/docker-ce/linux/ubuntu $(lsb_release -cs) stable"
# Step 4: 更新并安装 Docker-CE
sudo apt-get -y update
sudo apt-get -y install docker-ce
```

安装完毕后效果如下: 
查看版本
![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/8B8AA89DF747458CBAA67B3A0925CF19/195)


## 4. 设置阿里云docker加速服务

从docker官方的镜像服务器里面下载image非常慢, 使用阿里云的好处是可以拥有阿里的镜像加速服务, 速度可以达到百兆级别.
如果不配置也没有问题~ 多等一段时间就行了.

![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/9418AB5F29C24A3195155A9994471D2A/179)


ubuntu14.04系统采用不支持systemctl
>service docker restart  

重启docker服务


## 5. 安装hyperledger的工具和docker镜像

点击官方[参考文档](http://hyperledger-fabric.readthedocs.io/en/release-1.1/samples.html#binaries)

注意: 按照官方文档执行, 需要全局翻墙才行

![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/BB783CDD52B842A4A1BDAE19923FBA5E/225)


安装完检查目录结构和docker镜像:
![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/AC3A8997B51C4C65AF9E1D4CF7F3FCD1/232)


## 6. 下载官方的示例代码 fabric sample

>git clone https://github.com/hyperledger/fabric-samples.git

![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/DA434F1251A342F99E1BACC24C208524/240)



## 7. 切换到first-network目录

![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/0F0DA979549046F488AF91EDAC4D4CB5/247)



## 8. 启动fabric ledger的第一个网络
运行如下命令:
```
#1. 配置环境变量, fabirc的二进制工具
export PATH=/root/bin:$PATH
#2. 生成hyperledger fabric的各种区块链配置
./byfn.sh -m generate
#3. 安装docker-compose
curl -L https://github.com/docker/compose/releases/download/1.20.1/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
chmod +x /usr/local/bin/docker-compose
参考文档  https://github.com/docker/compose/releases
#4. 启动first-network
./byfn.sh -m up
```


## 9. 修复阿里云服务器网络错误的问题
(腾讯云不存在这个问题,自己装ubuntu也不存在这个问题)

![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/A34332D869CC472C97B03FEFACCECD66/272)

>/etc/resolv.conf 
注释掉 options timeout:2 attempts:3 rotate single-request-reopen
重新执行
```
./byfn.sh -m down
./byfn.sh -m up
```

就能够正常启动了.



## 10. 验证网络搭建
接下来你能看到,peer节点启动,通道建立, 加入通道, 设置锚节点,实例化链码,调用链码等一系列的操作.如果你看到下面的图,恭喜你!
你的网络搭建好了.
![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/BDA5E5DA03D14EF499B1D5896B673B19/283)

>看到上面的截图,说明你的开发环境已经准备好, 接下来我们就可以搭建自己的组织结构,编写nodejs的链码了.


## 11. 停止网络请使用命令

> ./byfn.sh -m down




## 12. 切换到basic-network目录
来到fabirc-sample目录的basic-network文件夹

## 13. 修改 basic-network的docker-compose.yml
![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/14483191ED1C41A7991A62E954BE79BD/298)
> 说明: 启用开发者模式,这样加快调试部署,减少资源开销
开启7052端口, 开发模式下不使用tls会减少出错的概率,生产环境需要启用tls




## 14. 修改 脚本 `start.sh`

![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/106A58A051F846CEACDE25E84C92C1B2/313)
增加cli节点, cli是方面我们执行控制指令的终端. 我们会使用他与各个peer节点进行交互.  后面这些手动的命令,会通过nodejs的api来调用



## 15. 启动脚本 'start.h'
![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/EAD0F21FA9394D5CB8E6394758C2BB26/326)
可以看到启动了ca节点,peer节点,order节点,cli节点和couchdb,创建了channel,peer加入了channel


## 16. 查看状态
![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/FAF8C3A10DF14B47B8AF757A6FBE8989/337)

```
docker ps
#可以看到当前运行的docker容器, peer,order,couchdb,ca,cli节点
docker exec -it bash 
#进入cli容器
peer channel list
#查看当前peer加入的channel
```


## 17. chaincode编写需要使用nodejs
请安装>8.0版本的nodejs

官网连接 [点我直达](https://nodejs.org/en/download/package-manager/#debian-and-ubuntu-based-linux-distributions)
```
curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
sudo apt-get install -y nodejs
```



## 18.编写nodejs的chaincode
```
#1.创建mycc文件夹
mkdir mycc
#2. 初始化package.json文件
npm init 
#3. 修改package.json文件
npm install --save fabric-shim --registry=https://registry.npm.taobao.org
```
成功后 目录结构如下:
终于package.json文件
![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/91A4EDDF81814883A9EE7572AB93DC6E/374)




## 19. 编写nodejs链码
```
const shim = require('fabric-shim');
const Chaincode = class{
    //链码初始化操作
    async Init(stub){
        var ret = stub.getFunctionAndParameters();
        var args  = ret.params;
        var a = args[0];
        var aValue = args[1];
        var b = args[2];
        var bValue = args[3];
        await  stub.putState(a,Buffer.from(aValue));
        await  stub.putState(b,Buffer.from(bValue));
        return shim.success(Buffer.from('heima chaincodinit successs'));
    }
    
    async Invoke(stub){
        let ret = stub.getFunctionAndParameters();
        let fcn = this[ret.fcn];
        return fcn(stub,ret.params);
    }
    //查询操作
    async query(stub,args){
        let a = args[0];
        let balance = await stub.getState(a);
        return shim.success(balance);
    }

};
shim.start(new Chaincode());
```


## 20. 把chaincode注册给peer
他们之间通过grcp协议通信
```
CORE_CHAINCODE_ID_NAME="mycc:v0"  npm start -- --peer.address grpc://192.168.0.1:7052
```
![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/94BCB93C2A2C4187865814C0844C15AD/390)



## 21. 在peer上install安装链码
这是peer上chaincode的生命周期

```
CORE_PEER_LOCALMSPID=Org1MSP CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp peer chaincode install -l node -n mycc -v v0 -p /opt/gopath/src/github.com/mycc/

```
![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/5BB063BA7E4449F2BCE3752F44F2FA86/406)



## 22. 在peer上实例化链码

```
CORE_PEER_LOCALMSPID=Org1MSP CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp peer chaincode instantiate -l node -n mycc -v v0 -C mychannel -c '{"args":["init","zzh","100","czbk","100"]}' -o 192.168.0.1:7050
```

![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/41BD7FC27F664B6D972E7C6BF048042F/413)


## 23. 测试链码调用


```
CORE_PEER_LOCALMSPID=Org1MSP CORE_PEER_MSPCONFIGPATH=/opt/gopath/src/github.com/hyperledger/fabric/peer/crypto/peerOrganizations/org1.example.com/users/Admin@org1.example.com/msp peer chaincode invoke -n mycc -C mychannel -c '{"args":["query","zzh"]}' -o 192.168.0.1:7050
```

![image](https://note.youdao.com/yws/public/resource/95c087db9c2f4249616a4058c521ca13/xmlnote/6E1AB1E615674B20AF6025CB40526D15/422)

可以查看到zzh账户上有100块钱.



## 24. 同理大家可以实现转账的操作.

试着自己实现一下transfer方法吧

## 25. 停止网络使用
> ./stop.sh ./teardown.sh


## 26. 查看环境是否清理干净
> docker ps 无内容就说明环境清理干净
```

```
