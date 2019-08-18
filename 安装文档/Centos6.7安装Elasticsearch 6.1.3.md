|        title         | categories |   tags   | date                |
| :------------------: | :--------: | :------: | ------------------- |
| Centos6.7安装ES6.1.3 |  fortest   | 数据开发 | 2019/08/18 19:21:21 |

> the life i want，there is not shortcut.

## 0x00前知

1，该ES6.1.3需要适配的JDK版本为1.8+

2，本文安装前JDK环境已配置好，如无配置，请自行谷歌并配置好JDK环境

3，该ES6.1.3不能使用root用户启动，所以需要新建一个elasticsearch用户执行ES所在目录，本文使用elsearch

![](E:\GitHub\Big-data-groping\images_all_in_here\安装文档\ES6.1.3\步骤一添加修改用户.png)

## 0x01开始安装

1，解压 tar -zxf esxxx -C /opt/app

2，chown -R elsearch:elsearch esxxx

3，修改配置文件 cd /esxxx/config   vi elasticsearch.yml       "[]"内为提示内容，不是修改的内容，谨记

```
1,修改cluster.name: cluster.name: my-test-application
2,修改node.name: node-1
3,修改path.data: /data/elsearch6/data   [记得创建此目录，并且修改所属用户/组为elsearch]
4,修改path.logs: /data/elsearch6/logs   [记得创建此目录，并且修改所属用户/组为elsearch]
5,在Memory一栏添加bootstrap.system_call_filter: flase 并将bootstrap.memory_lock: false  [ES5之后默认为true,检测失败报错]
6,修改network.host: 192.168.91.163[本机的ip]
7,末尾添加[head使用，如未有head或者不打算安装head,可略过此处]
http.cors.enabled: true
http.cors.allow-origin: "*"
8,scp分发后修改各自的node和ip

```

## 0x02异常

1，max number of threads [2048] for user [elsearch] is too low, increase to at least [4096]

```
vi /etc/security/limits.d/90-nproc.conf 
* soft nofile 65536
* hard nofile 131072
* soft nproc 4096
* hard nproc 4096

如果无效，修改 vi /etc/security/limits.conf 修改elsearch的limit
elsearch hard nofile 131072
elsearch soft nproc 4096
elsearch soft nofile 65536
```

## 0x03安装head

1，需要node，去node官网下载node，如果下载的是XZ格式的还需要安装XZ格式的解压器[yum install xz]

2，配置node的环境变量

3，安装npm

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

4，使用npm安装grunt

```
npm install -g grunt

npm install -g grunt-cli --registry=https://registry.npm.taobao.org --no-proxy
```

5，确认是否安装成功

```
node -v
v10.16.0
```

6，下载head插件源码

```
 wget https://github.com/mobz/elasticsearch-head/archive/master.zip
 
 uzip msater.zip
```

7，依赖

```
进入上面解压后的目录，执行以下命令
npm install

如果网速过慢失败--可以使用下面的
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

8，结合ES

```
1,先停掉ES
2,修改es的配置文件[前面的那个]
添加--注意":"后面有一个空格
http.cors.enabled: true
http.cors.allow-origin: "*"

2,修改head中的文件 vi Gruntfile.js
找到connect:Server,添加hostname
connect: {
                        server: {
                                options: {
                                        hostname: '0.0.0.0',
                                        port: 9100,
                                        base: '.',
                                        keepalive: true
                                }
                        }
                }
hostname改为'0.0.0.0'和'*'均可，看应用场景
```

9，启动

```
先启动ES
bin/elasticsearch -d

然后在head目录下启动 
nohup npm run start &

```

10，访问

打开http://192.168.91.163:9100/

## 0x04安装IK

支持两种形式的安装

1，以plugin方式安装

```
进入ES的bin路径，执行以下命令：
elasticsearch-plugin install https://github.com/medcl/elasticsearch-analysis-ik/releases/download/v6.1.3/elasticsearch-analysis-ik-6.1.3.zip

执行完毕，重启ES，ik安装完毕
```

2，手动安装

```
：注意版本号要求一致
1,在 https://github.com/medcl/elasticsearch-analysis-ik/releases下载这个版本的IK和Es的版本对应关系一致，本文使用es 6.1.3，下载后
2，解压到ES_HOME/plugins/ik 目录下面(直接包含一个conf文件夹和一堆.jar包)
3，重新启动ES
4，如果 看到try load config ....IK相关信息，说明启动完成后和安装IK插件完成。或者访问http://spark01:9200/_cat/plugins  出现node-1 analysis-ik 6.1.3即为完成安装
```

3，验证

```
curl -H "Content-Type: application/json" -XPOST "http://spark01:9200/_analyze?pretty=true" -d'{"text":"helloworld,我是一个粉刷匠"}'

{
  "tokens" : [
    {
      "token" : "helloworld",
      "start_offset" : 0,
      "end_offset" : 10,
      "type" : "<ALPHANUM>",
      "position" : 0
    },
    {
      "token" : "我",
      "start_offset" : 11,
      "end_offset" : 12,
      "type" : "<IDEOGRAPHIC>",
      "position" : 1
    },
    {
      "token" : "是",
      "start_offset" : 12,
      "end_offset" : 13,
      "type" : "<IDEOGRAPHIC>",
      "position" : 2
    },
    {
      "token" : "一",
      "start_offset" : 13,
      "end_offset" : 14,
      "type" : "<IDEOGRAPHIC>",
      "position" : 3
    },
    {
      "token" : "个",
      "start_offset" : 14,
      "end_offset" : 15,
      "type" : "<IDEOGRAPHIC>",
      "position" : 4
    },
    {
      "token" : "粉",
      "start_offset" : 15,
      "end_offset" : 16,
      "type" : "<IDEOGRAPHIC>",
      "position" : 5
    },
    {
      "token" : "刷",
      "start_offset" : 16,
      "end_offset" : 17,
      "type" : "<IDEOGRAPHIC>",
      "position" : 6
    },
    {
      "token" : "匠",
      "start_offset" : 17,
      "end_offset" : 18,
      "type" : "<IDEOGRAPHIC>",
      "position" : 7
    }
  ]
}
```

