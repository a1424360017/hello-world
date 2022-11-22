
# 请使用 Go/C++ 实现 (难度 1.5)

## 环境准备

主机 SSH 地址: 81.70.3.124 用户名: ubuntu 密码: 9b1deb4d-2b0d7b3dcb6d

* 安装和配置 go/cpp 开发、运行时环境(任选其一)
* 安装 redis-server(关闭 AUTH)，启动


## 1. redis RESP 实现 (阅读理解,TCP,迭代)

参考 redis [RESP 通信规范](https://redis.io/docs/reference/protocol-spec/), 实现如下功能:

* 建立 tcp 连接到 redis-server
* 实现 RESP send/recv 核心逻辑
* 实现 SET(demo-key, demo-value)
* 实现 GET(demo-key)


提示

* 仅使用 go 或 cpp 官方标准库，不依赖第三方库或者框架，协议 parse/format 实现简单高效
* 可以参考其它开源项目实现方式，迅速看懂代码，能把有用的部分复制出来二次修改实现


## 2. 查找 db key size 中值

leveldb 是一个按照 key 有序排序的数据存储库:

* cpp [leveldb](https://github.com/google/leveldb)
* golang [goleveldb](https://github.com/syndtr/goleveldb)

现基于它实现如下功能:

* 创建一个基于本地文件系统的 KV 实例，创建样本数据, 要求 key 值长度在 16 ~ 64 bytes 之间随机，value 值长度在 1 ~ 1024 之间随机，样本总数默认 MAX_SAMPLE_NUM = 100000 
* 基于 goleveldb.SizeOf() 或 leveldb.GetApproximateSizes() 方法实现一个函数 GetMiddleKey()，要求返回的 middleKey 可以将 db size 整体左右均分 (假设误差 abs(leftSize - rightSize) / dbSize < 0.01)


## 3. 空间数据索引、检索

有如下数据样例:

```
id(string), x(double), y(double)
0001, 0.1, 0.2
0002, 1.1, 1.2
0003, 2.0, 3.0
```

表示物体(id) 所在空间的坐标点(x, y, 单位米), 要求:

* 设计一种基于内存的数据结构，索引以上数据，写入接口如: Insert(const string &id, double x, double y)
* 设计一个方法用于检索指定坐标点半径以内的物体 id 列表,如 std::vector<string> Query(double x, double y, double radius)

