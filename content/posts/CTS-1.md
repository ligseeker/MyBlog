# CTS-1

> 咖喱铁路售票系统curry ticketing system -1

[![bqal7T.png](https://s1.ax1x.com/2022/03/13/bqal7T.png)](https://imgtu.com/i/bqal7T)

## 题目背景

​		在世界上某个能歌善舞的国家，铁路系统实行配额制，即为弱势群体提供不同的票价优惠，人们通过实名制进行铁路火车票预订，可以查询线路图和购票情况。铁路管理员则负责管理铁路线路，火车营运。同时，还存在团购性质的代理人，能够一次性购买更多的车票。为了让大家能够有序购票乘车，现开发一款铁路售票管理系统，实现上述场景功能。



## 功能描述

### ① 开关机

实现最基本的命令读入和退出功能

首先，你的任务是编写一个Test类：

- 当程序启动，进入main方法，并连续读入输入的命令，命令的基本格式为：

  ```
  选项 [参数1] [参数2] [参数3]
  ```

  其中参数数目为不定项

- 当终端输入 QUIT 时，系统退出（程序退出状态为0），并在终端打印一行字符：

```shell
----- Good Bye! -----
```

- 对于其他的输入，不做任何处理，等待下一行输入
- 下方是按行读取的一种参考实现

```java
Scanner in = new Scanner(System.in);
String argStr;
while (true) {
    argStr = in.nextLine();
}
```



### ②用户注册

黄金右手国的居民们使用的身份证称为“Aadhaar卡"，铁路系统内的用户注册需要填写姓名、性别、Aadhaar卡号。其中Aadhaar号需要满足以下格式：

[![bqaXD0.jpg](https://s1.ax1x.com/2022/03/13/bqaXD0.jpg)](https://imgtu.com/i/bqaXD0)

1. Aadhaar卡号一共12位，全国唯一
2. 前4位为区域代码，范围是[0001,1237]
3. 中间4位为种姓代码，范围为[0020,0460]
4. 最后四位为生物识别库提供的生物识别码，前三位的范围为[000,100]，最后一位表示持有人的性别，0代表女性，1代表男性，2代表其他。

**合法的Aadhaar卡号示例：**

+ 0023 0122 0991
+ 1000 0072 0000

**不合法的Aadhaar卡号示例：**

+ 0000 0122 0991
+ 0030 0500 1010
+ 1234 0123 0553



**具体实现**

为用户设立一个 User 类：

+ 私有属性至少包括姓名（仅可能由 26 个字母大小写和下划线构成）、性别（字符 M/F/O，分别表示男性/女性/其他）、Aadhaar卡号（符合上述合法格式）

+ 为用户的3种属性提供相应的 `getter` 和 `setter` 方法

+ 实现方法 `String toString()` 打印用户格式化信息，具体要求如下：

  - 冒号为英文字符 `:`
  - 不包含多余的空格
  - 所有字符为全角字符

  以 sb_DYY，Aadhaar卡号 0910 0072 0112 为例：

```
Name:sb_DYY
Sex:O
Aadhaar:091000720112 
```



+ 实现添加用户方法，命令格式如下：

| 选项    | [参数 1] | [参数 2] | [参数 3] | 功能描述                                                     |
| ------- | -------- | -------- | -------- | ------------------------------------------------------------ |
| addUser | 姓名     | 性别     | 卡号     | 新增用户对象并储存相关信息，对于非法输入，终端输出相应报错。对于合法输入，调用该对象的 `toString()` 方法。 |



具体要求如下：

- 在输入 addUser 命令时，并不保证参数数量严格对应，这也是一种非法情况，需要在终端输出：

  ```shell
  Arguments illegal
  ```

- 如果参数合法，按以下顺序依次进行信息检查

  - 姓名由 26 个字母和下划线构成，其他情况请输出

  ```
  Name illegal
  ```

  - 性别必须为 F / M / O，其他情况请输出

  ```shell
  Sex illegal
  ```

  - 卡号需满足指定格式，非法（包括尾号和性别不符的情况）请输出

  ```shell
  Aadhaar number illegal
  ```

  + 一张卡号只能注册一次，若注册卡号已存在请输出

  ```
  Aadhaar number exist
  ```

  

- 如果存在多种非法情况，按上述顺序只输出最先发生的非法信息。

+ 如果输入合法，按 `toString()` 输出格式打印刚添加的用户信息。如：

```shell
addUser LaoWang M
Arguments illegal
addUser LaoWang GHS 123123
Sex illegal
addUser LaoWang M 003005001010
Aadhaar number illegal
addUser LaoWang M 002301220991
Name:LaoWang
Sex:M
Aadhaar:002301220991
addUser HaiWang M 002301220991
Aadhaar number exist
```

