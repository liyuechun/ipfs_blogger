# 【IPFS + 区块链 系列】 入门篇 - IPFS+IPNS+个人博客搭建

> 孔壹学院：国内区块链职业教育引领品牌。
> 
> 作者：黎跃春，孔壹学院创始人，区块链、高可用架构师
> 
> 微信：liyc1215
> 
> 区块链博客：http://liyuechun.org

在阅读这篇文章之前，你需要先学习[【IPFS + 区块链 系列】 入门篇 - IPFS环境配置](http://liyuechun.org/2017/11/20/ipfs-blockchain/)这篇文章。

## 目录


- [1. 如何在IPFS新增一个文件](#1-如何在IPFS新增一个文件)
    - [1.1 新建file.txt文件](#11-新建file.txt文件)
    - [1.2 查看ipfs相关命令](#12-查看ipfs相关命令)
    - [1.3 将file.txt添加到ipfs节点](#13-将file.txt添加到ipfs节点)
- [2. 通过ipfs创建目录存储文件](#2-通过ipfs创建目录存储文件)
- [3. 如何在IPFS新增一个目录](#3-如何在IPFS新增一个目录)
    - [3.1 使用ipfs add -r可以上传一整个目录](#31-使用ipfs add -r可以上传一整个目录)
    - [3.2 通过路径访问contactme.txt文件数据](#32-通过路径访问contactme.txt文件数据)
    - [3.3 通过Hash查看数据IPFS网络数据](#33-通过Hash查看数据IPFS网络数据)
- [4. 创建简易的网页发布到IPFS](#4-创建简易的网页发布到IPFS)
    - [4.1 创建一个index.html文件](#41-创建一个index.html文件)
    - [4.2 创建一个style.css文件](#42-创建一个style.css文件)
    - [4.3 添加到ipfs](#43-添加到ipfs)
    - [4.4 网络同步](#44-网络同步)
    - [4.5 访问网站](#45-访问网站)
    - [4.6 发布到IPNS](#46-发布到IPNS)
- [5. 发布个人博客](#5-发布个人博客)
    - [5.1 搭建静态博客](#51-搭建静态博客)
    - [5.2 节点ID替换](#52-节点ID替换)
    - [5.3 浏览博客](#53-浏览博客)
- [6. 下篇预报](#6-下篇预报)
    - [6.1 `ipfs + ethereum``Dapp`开发入门](#61-`ipfs + ethereum``Dapp`开发入门)


## 1. 如何在IPFS新增一个文件

### 1.1 新建file.txt文件

打开终端，切换到桌面，新建一个文件夹`1121`，切换到`1121`中，通过`vi`新建一个文件`file.txt`，文件里面输入春哥微信号`liyc1215`保存并且退出。

```
localhost:Desktop yuechunli$ pwd
/Users/liyuechun/Desktop
localhost:Desktop yuechunli$ mkdir 1121
localhost:Desktop yuechunli$ cd 1121/
localhost:1121 yuechunli$ vi file.txt
localhost:1121 yuechunli$ cat file.txt 
liyc1215
localhost:1121 yuechunli$ 
```

### 1.2 查看ipfs相关命令

```
localhost:1121 yuechunli$ ipfs help
USAGE
  ipfs - Global p2p merkle-dag filesystem.

  ipfs [--config=<config> | -c] [--debug=<debug> | -D] [--help=<help>] [-h=<h>] [--local=<local> | -L] [--api=<api>] <command> ...

SUBCOMMANDS
  BASIC COMMANDS
    init          Initialize ipfs local configuration
    add <path>    Add a file to IPFS
    cat <ref>     Show IPFS object data
    get <ref>     Download IPFS objects
    ls <ref>      List links from an object
    refs <ref>    List hashes of links from an object
  
  DATA STRUCTURE COMMANDS
    block         Interact with raw blocks in the datastore
    object        Interact with raw dag nodes
    files         Interact with objects as if they were a unix filesystem
    dag           Interact with IPLD documents (experimental)
  
  ADVANCED COMMANDS
    daemon        Start a long-running daemon process
    mount         Mount an IPFS read-only mountpoint
    resolve       Resolve any type of name
    name          Publish and resolve IPNS names
    key           Create and list IPNS name keypairs
    dns           Resolve DNS links
    pin           Pin objects to local storage
    repo          Manipulate the IPFS repository
    stats         Various operational stats
    p2p           Libp2p stream mounting
    filestore     Manage the filestore (experimental)
  
  NETWORK COMMANDS
    id            Show info about IPFS peers
    bootstrap     Add or remove bootstrap peers
    swarm         Manage connections to the p2p network
    dht           Query the DHT for values or peers
    ping          Measure the latency of a connection
    diag          Print diagnostics
  
  TOOL COMMANDS
    config        Manage configuration
    version       Show ipfs version information
    update        Download and apply go-ipfs updates
    commands      List all available commands
```

### 1.3 将file.txt添加到ipfs节点

```
localhost:1121 yuechunli$ ls
file.txt
localhost:1121 yuechunli$ ipfs add file.txt 
added QmbrevseVQKf1vsYMsxCscRf6D7S2dftYpHwxkYf94pc7T file.txt
localhost:1121 yuechunli$ cat file.txt 
liyc1215
localhost:1121 yuechunli$ ipfs cat QmbrevseVQKf1vsYMsxCscRf6D7S2dftYpHwxkYf94pc7T
liyc1215
localhost:1121 yuechunli$ 
```

当执行完`ipfs add file.txt`这个命令以后，会将`file.txt`添加到`ipfs`当前的节点中，并且会对`file.txt`文件生成一个唯一的`hash``QmbrevseVQKf1vsYMsxCscRf6D7S2dftYpHwxkYf94pc7T`，如果想查看本地`ipfs`节点的数据，可以通过`ipfs cat QmbrevseVQKf1vsYMsxCscRf6D7S2dftYpHwxkYf94pc7T`进行查看。

**⚠️：**当我试图通过`http://ipfs.io/ipfs/QmbrevseVQKf1vsYMsxCscRf6D7S2dftYpHwxkYf94pc7T`进行数据访问时，无法访问，如下图所示：

![](http://om1c35wrq.bkt.clouddn.com/fangwen.gif)

**⚠️：**虽然数据已经添加到当前的你自己的`IPFS`节点中，但是并没有同步到`IPFS`网络，所以暂时在网络上无法访问。

**⚠️：重要：接下来执行下面的命令同步节点数据到`IPFS`网络，再试图在网络上查看数据。**

- 同步节点

新建一个终端，执行`ipfs daemon`。

```
localhost:.ipfs yuechunli$ ipfs daemon
Initializing daemon...
Adjusting current ulimit to 2048...
Successfully raised file descriptor limit to 2048.
Swarm listening on /ip4/111.196.246.151/tcp/3637
Swarm listening on /ip4/127.0.0.1/tcp/4001
Swarm listening on /ip4/169.254.170.167/tcp/4001
Swarm listening on /ip4/192.168.0.107/tcp/4001
Swarm listening on /ip6/::1/tcp/4001
API server listening on /ip4/127.0.0.1/tcp/5001
Gateway (readonly) server listening on /ip4/127.0.0.1/tcp/8080
Daemon is ready
```

- 从`IPFS`网络查看数据

浏览器访问[https://ipfs.io/ipfs/QmbrevseVQKf1vsYMsxCscRf6D7S2dftYpHwxkYf94pc7T](https://ipfs.io/ipfs/QmbrevseVQKf1vsYMsxCscRf6D7S2dftYpHwxkYf94pc7T)

![黎跃春 微信](http://om1c35wrq.bkt.clouddn.com/WX20171121-105531@2x.png)


## 2. 通过ipfs创建目录存储文件

在着上面的步骤走，我们可以通过`ipfs cat QmbrevseVQKf1vsYMsxCscRf6D7S2dftYpHwxkYf94pc7T
liyc1215`查看添加到`ipfs`网络的`file.txt`文件的内容，如下：

```
localhost:1121 yuechunli$ ipfs cat QmbrevseVQKf1vsYMsxCscRf6D7S2dftYpHwxkYf94pc7T
liyc1215
localhost:1121 yuechunli$ 
```

当然，我们也可以通过`ipfs`的相关命令在`ipfs`的根目录下面创建文件夹，并且将`file.txt`文件**移动**或者**拷贝**到我们创建的文件夹中。

**⚠️：cp不会改变文件hash，mv会改变hash寻址。**

```
localhost:1121 yuechunli$ ipfs cat QmbrevseVQKf1vsYMsxCscRf6D7S2dftYpHwxkYf94pc7T
liyc1215
localhost:1121 yuechunli$ ipfs files mkdir /LiYueChun
localhost:1121 yuechunli$ ipfs files cp /ipfs/QmbrevseVQKf1vsYMsxCscRf6D7S2dftYpHwxkYf94pc7T /LiYueChun/file.txt
localhost:1121 yuechunli$ ipfs files ls /
LiYueChun
localhost:1121 yuechunli$ ipfs files ls /LiYueChun/
file.txt
localhost:1121 yuechunli$ ipfs files read /LiYueChun/file.txt
liyc1215
localhost:1121 yuechunli$ 
```



## 3. 如何在IPFS新增一个目录

### 3.1 使用ipfs add -r可以上传一整个目录

```
localhost:1121 yuechunli$ ipfs add -r ipfs-tutorial/
added QmYx4BnhnLXeMWF5mKu16fJgUBiVP7ECXh7qcsUZnXiRxc ipfs-tutorial/contactme.txt
added QmfKdWsguobA3aDPvSxLB3Bq4HMKyqKSgFr2NFUuVH8n31 ipfs-tutorial/eth-fabric.png
added QmXe8jTxTh5MZP6BK5cnj19mXNTKVMzNyUJZUHuYyr5dk1 ipfs-tutorial/gongzhonghao.png
added QmSsjQDVw1fvmG5RsZMgp2GjihiXn2zDv64mfHZN3AREek ipfs-tutorial
```

#### 3.2 通过路径访问contactme.txt文件数据

如果我们上传的是目录，那么可以通过下面几种方式访问到`contactme.txt`文件的数据。

```
localhost:1121 yuechunli$ ipfs cat QmYx4BnhnLXeMWF5mKu16fJgUBiVP7ECXh7qcsUZnXiRxc
微信：liyc1215
区块链技术交流群：348924182
公众号：区块链部落
localhost:1121 yuechunli$ ipfs cat /ipfs/QmYx4BnhnLXeMWF5mKu16fJgUBiVP7ECXh7qcsUZnXiRxc
微信：liyc1215
区块链技术交流群：348924182
公众号：区块链部落
localhost:1121 yuechunli$ ipfs cat /ipfs/QmSsjQDVw1fvmG5RsZMgp2GjihiXn2zDv64mfHZN3AREek/contactme.txt
微信：liyc1215
区块链技术交流群：348924182
公众号：区块链部落
localhost:1121 yuechunli$ 
```

### 3.3 通过Hash查看数据IPFS网络数据

- **访问目录：**[https://ipfs.io/ipfs/QmSsjQDVw1fvmG5RsZMgp2GjihiXn2zDv64mfHZN3AREek](https://ipfs.io/ipfs/QmSsjQDVw1fvmG5RsZMgp2GjihiXn2zDv64mfHZN3AREek)

![](http://om1c35wrq.bkt.clouddn.com/WX20171121-110959@2x.png)

- **通过目录访问文件：**[https://ipfs.io/ipfs/QmSsjQDVw1fvmG5RsZMgp2GjihiXn2zDv64mfHZN3AREek/contactme.txt](https://ipfs.io/ipfs/QmSsjQDVw1fvmG5RsZMgp2GjihiXn2zDv64mfHZN3AREek/contactme.txt)
![](http://om1c35wrq.bkt.clouddn.com/WX20171121-111019@2x.png)

- **通过文件hash直接访问：**[https://ipfs.io/ipfs/QmYx4BnhnLXeMWF5mKu16fJgUBiVP7ECXh7qcsUZnXiRxc](https://ipfs.io/ipfs/QmYx4BnhnLXeMWF5mKu16fJgUBiVP7ECXh7qcsUZnXiRxc)

![](http://om1c35wrq.bkt.clouddn.com/WX20171121-113047@2x.png)


## 4. 创建简易的网页发布到IPFS


在这里我先自己写一个简单的网页给大家演示，先在桌面新建一个`site`文件夹，然后按照下面的步骤在`site`文件夹中建立`index.html`和`style.css`文件。

### 4.1 创建一个index.html文件

```
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <title>Hello IPFS!</title>
  <link rel="stylesheet" href="./style.css" />
</head>
<body>
  <h1>Hello IPFS!</h1>
</body>
</html>
``` 

### 4.2 创建一个style.css文件

```
h1 {
  color: green;
}
```

### 4.3 添加到ipfs

```
localhost:Desktop yuechunli$ ipfs add -r site/
added QmWG5rbgT9H77TGq49RXNoqN8M7DNKMnMX425nkmCB6BjS site/index.html
added QmfGLJ3mryLvicQqzdsghq4QRhptKJtBAPe7yDJxsBGSuy site/style.css
added QmdVEGkT5u7LtzzatTrn8JGNEF3fpuMPVs2rPCfvqRykRp site
```

最后一行是项目根目录的`hash`，你先通过`ipfs daemon`同步网络，然后可以通过`https://ipfs.io/ipfs/<你的项目根目录hash>`，即`https://ipfs.io/ipfs/QmdVEGkT5u7LtzzatTrn8JGNEF3fpuMPVs2rPCfvqRykRp`访问项目。

### 4.4 网络同步

```
localhost:Desktop yuechunli$ ipfs daemon
```

### 4.5 访问网站

浏览器打开[https://ipfs.io/ipfs/QmdVEGkT5u7LtzzatTrn8JGNEF3fpuMPVs2rPCfvqRykRp](https://ipfs.io/ipfs/QmdVEGkT5u7LtzzatTrn8JGNEF3fpuMPVs2rPCfvqRykRp)，效果图如下：

![](http://om1c35wrq.bkt.clouddn.com/WX20171121-210721@2x.png)

### 4.6 发布到IPNS

当我们修改网站内容重新添加到`ipfs`时，`hash`会发生变化，当我们网站更新时，我们可以将网站发布到IPNS，在IPNS中，允许我们节点的域名空间中引用一个`IPFS hash`，也就是说我们可以通过节点`ID`对项目根目录的`IPFS HASH`进行绑定，以后我们访问网站时直接通过节点·ID`访问即可，当我们更新博客时，重新发布到`IPNS`即可。

```
localhost:~ yuechunli$ ipfs name publish QmdVEGkT5u7LtzzatTrn8JGNEF3fpuMPVs2rPCfvqRykRp
Published to QmdKXkeEWcuRw9oqBwopKUa8CgK1iBktPGYaMoJ4UNt1MP: /ipfs/QmdVEGkT5u7LtzzatTrn8JGNEF3fpuMPVs2rPCfvqRykRp
localhost:~ yuechunli$ ipfs id
{
	"ID": "QmdKXkeEWcuRw9oqBwopKUa8CgK1iBktPGYaMoJ4UNt1MP"
}
```

当我们执行`ipfs name publish`命令时，会返回我们的节点`ID`，你可以通过`ipfs id`进行查看验证是否是你的节点`ID`。

**⚠️：验证**

```
$ ipfs name resolve <peerId>
```

```
localhost:~ yuechunli$ ipfs name resolve QmdKXkeEWcuRw9oqBwopKUa8CgK1iBktPGYaMoJ4UNt1MP
/ipfs/QmdVEGkT5u7LtzzatTrn8JGNEF3fpuMPVs2rPCfvqRykRp
localhost:~ yuechunli$ 
```

**⚠️：**当然我们现在就可以通过`IPNS`进行访问了。

```
https://ipfs.io/ipns/QmdKXkeEWcuRw9oqBwopKUa8CgK1iBktPGYaMoJ4UNt1MP
```

**⚠️⚠️⚠️：注意上面是ipns而不是ipfs。**

![](http://om1c35wrq.bkt.clouddn.com/WX20171121-212123@2x.png)


**⚠️：如果你网站数据修改，需要重新发布到IPNS。**

## 5. 发布个人博客

你可以通过`Hugo`按照官方文档创建一个漂亮的静态博客[Hugo官方网站](http://gohugo.io/getting-started/quick-start/)，当然你也可以自己编写，或者使用其他开源项目搭建。

### 5.1 搭建静态博客

大家可以自己搭建，也可以直接下载我的博客源码直接搭建。

源码地址：[http://github.com/liyuechun/ipfs_blogger](http://github.com/liyuechun/ipfs_blogger)


### 5.2 节点ID替换

- 查看你的节点ID

```
localhost:ipfs_pin yuechunli$ ipfs id
{
	"ID": "《your peer id》"
}
localhost:ipfs_pin yuechunli$ 
```

在上面的源码中全局搜索将源码里面的`QmdKXkeEWcuRw9oqBwopKUa8CgK1iBktPGYaMoJ4UNt1MP`替换成你自己的`ID`。

接下来重复[4. 创建简易的网页发布到IPFS](#4-创建简易的网页发布到IPFS)的操作步骤即可。

### 5.3 浏览博客

浏览器打开[https://ipfs.io/ipns/QmdKXkeEWcuRw9oqBwopKUa8CgK1iBktPGYaMoJ4UNt1MP/](https://ipfs.io/ipns/QmdKXkeEWcuRw9oqBwopKUa8CgK1iBktPGYaMoJ4UNt1MP/)查看项目效果。

![IPFS 博客项目效果图](http://om1c35wrq.bkt.clouddn.com/WX20171121-215636@2x.png)

## 6. 下篇预报

### 6.1 `ipfs + ethereum``Dapp`开发入门

## 7. 技术交流

- 区块链技术交流QQ群：`348924182`
- 进微信群请加微信：`liyc1215`
- 「区块链部落」官方公众号

![](http://om1c35wrq.bkt.clouddn.com/%E5%8C%BA%E5%9D%97%E9%93%BE%E9%83%A8%E8%90%BD.png)


