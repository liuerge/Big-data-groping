|       title        | categories |   tags   | date                |
| :----------------: | :--------: | :------: | ------------------- |
| ES面试相关的知识点 |  fortest   | 数据开发 | 2019/08/14 19:21:21 |

> the life i want，there is not shortcut.

## 0x00ES

1，ES如何避免脑裂

2，match，term的区别

3，介绍下ES的索引流程，分片

4，ES索引和Lucene的区别

5，Kiban怎么分析，制作报表

6，Logstash怎么解析原始的Ngnix日志

```
1，用正则
2，grok

input {stdin{}}
filter {
    grok {
        match => {
            "message" => "\s+(?<request_time>\d+(?:\.\d+)?)\s+"
        }
    }
}
output {stdout{}}
```

