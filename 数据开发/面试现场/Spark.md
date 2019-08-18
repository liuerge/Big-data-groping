|         title         | categories |   tags   | date                |
| :-------------------: | :--------: | :------: | ------------------- |
| Spark面试相关的知识点 |  fortest   | 数据开发 | 2019/08/13 19:21:21 |

> the life i want，there is not shortcut.

## 0x00Spark

1，Spark sql运行的原理

2，Spark2.x和1.6版本的区别

3，ds和df的区别

4，hashmanager和sortedmanager的区别，为什么性能好 钨丝为啥好

5，任务提交流程

6，对源码的理解

7，groupbykey 和reducebykey

8，spark分区的划分规则

9，Spark实现矩阵线程

10，Spark实现wordcount

11，列举一些action算子和transform算子 ，底层实现

12，手写-查找二度好友

13，提交资源分配，yan-cluster会问队列相关，长时间运行，会问相关参数

14，Spark内存模型

15，Spark任务调度的源码

16，spark里kyro序列化了解多少

17，给你一个用户信息包（包含用户的各种id），如何与你公司已有的用户之间进行匹配，使用高效的方式？[BitMap]

18，如何保证累加器的精准性？

19，SS窗口大小，每个窗口的数据，从Kafka中读取的events多少

20，讲一下checkpoint，相比Flink呢

21，Spark shuffle map端溢写的数据结构

22，Spark算一个分钟在线人数的topk [已有用户id，用户一个session的登入和登出时间]

23，Spark算子做连接操作时，join与cogroup的区别

```
join就是把两个集合根据key,进行内容聚合;

cogroup就是:有两个元组Tuple的集合A与B,先对A组集合中key相同的value进行聚合
		   然后对B组集合中key相同的value进行聚合,之后对A组与B组进行"join"操作
```

24，