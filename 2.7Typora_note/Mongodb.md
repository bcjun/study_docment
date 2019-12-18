# Mongodb数据库

- 可执行文件存放路径：`/usr/bin/mongod` 和 `/usr/bin/mongo`
- 数据库文件存放路径：`/data/db`
- 日志文件存放路径：`/var/log/mongodb/mongod.log`
- 配置文件存放路径：`/etc/mongod.conf`

> ~~~
> 启动MongoDB有2种方式，一种是直接默认启动，另一种是指定配置文件。启动方式如下：
> 
> 1:  /etc/init.d/mongod start 或service mongod start 
> 
> 2:  mongod --config /etc/mongodb.conf	可以自行配置
> 启动: sudo service mongod start
> 停止: sudo service mongod stop
> 重启: sudo service mongod restart
> ~~~

> ~~~
> 
> ~~~
>
> 

### Mongodb的**权限**管理

### Mongodb数据库操作

```
键默认为string模式
```

> **切换数据库**
>
> ```
> use admin
> ```
>
> **创建超级用户**
>
> ```
> # 超级用户必须再admin数据库中创建
> use admin
> db.createUser({"user":"python","pwd":"python","roles":["root"]})
> ```
>
> **用户登录**
>
> ```
> use admin
> db.auth('python','python')
> ```
>
> **查看用户**
>
> ```
> show users
> ```
>
> **查看数据库**
>
> ```
> show databases
> ```
>
> **查看当前所在数据库**
>
> ```
> db
> ```
>
> **删除用户**
>
> ```
> db.dropUser('python')
> ```

# Mongodb集合部分

### mongodb集合的命令

> **查看当前数据库集合**
>
> ```
> show collections
> show tables
> ```
>
> **查看集合所有字段**
>
> ```
> db.集合名称.find()
> ```
>
> 
>
> **删除集合**
>
> ```
> db.集合名称.drop()
> ```
>
> **手动创建结合**
>
> ```python
> # 不手动创建集合： 向不存在的集合中第⼀次加⼊数据时， 集合会被创建出来
> db.createCollection(name,options)
> db.createCollection("stu")
> db.createCollection("sub", { capped : true, size : 10 } )
> 参数capped： 默认值为false表示不设置上限,值为true表示设置上限
> 参数size： 当capped值为true时， 需要指定此参数， 表示上限⼤⼩,当⽂档达到上限时， 会将之前的数据覆盖， 单位为字节
> ```
>
> **删除集合**
>
> ```
> db.集合名称.drop()
> ```
>
> **检查集合是否有上限**
>
> ```
> db.集合名.isCapped()
> ```

### mongodb中常见的数据类型

| 数据类型  | 释义                                  |
| --------- | ------------------------------------- |
| Object ID | ⽂档ID                                |
| String    | 字符串， 最常⽤， 必须是有效的UTF-8   |
| Boolean   | 存储⼀个布尔值， true或false          |
| Integer   | 整数可以是32位或64位， 这取决于服务器 |
| Double    | 存储浮点值                            |
| Arrays    | 数组或列表， 多个值存储到⼀个键       |
| Object    | ⽤于嵌⼊式的⽂档， 即⼀个值为⼀个⽂档 |
| Null      | 存储Null值                            |
| Timestamp | 时间戳， 表示从1970-1-1到现在的总秒数 |
| Date      | 存储当前⽇期或时间的UNIX时间格式      |

```
每个⽂档都有⼀个属性， 为_id， 保证每个⽂档的唯⼀性，mongodb默认使用_id作为主键

可以⾃⼰去设置_id插⼊⽂档，如果没有提供， 那么MongoDB为每个⽂档提供了⼀个独特的_id， 类型为objectID

objectID是⼀个12字节的⼗六进制数,每个字节两位，一共是24 位的字符串： 前4个字节为当前时间戳 接下来3个字节的机器ID 接下来的2个字节中MongoDB的服务进程id 最后3个字节是简单的增量值
```

# mongodb的增删改

**mongodb的插入**

> db.集合名称.insert(document)

**mongodb的保存**

> db.集合名称.save(document)
>
> ```
> 如果⽂档的_id已经存在则修改， 如果⽂档的_id不存在则添加
> db.stu.save({_id:5,age:59,name:"黄忠"})
> 如果_id存在则修改，不存在则新建
> ```

**mongodb的简单查询**

> db.集合名称.find()
>
> ```
> db.stu.find({age:18})
> ```
>
> 

**mongodb的更新**

> db.集合名称.update(<query> ,<update>,{multi: <boolean>})
>
> ```
> 参数query:查询条件
> 参数update:更新操作符
> 参数multi:可选， 默认是false，表示只更新找到的第⼀条记录， 值为true表示把满⾜条件的⽂档全部更新
> 
> db.stu.update({name:'hr'},{name:'mnc'})           全文档进行覆盖更新，只更新一条数据
> db.stu.update({name:'hr'},{$set:{name:'hys'}})    指定键值更新操作
> db.stu.update({},{$set:{gender:0}},{multi:true})   更新全部
> ```
>
> 

**mongodb的删除**

> db.集合名称.remove(<query>,{justOne: <boolean>})
>
> ```
> 参数query:可选，删除的⽂档的条件
> 参数justOne:可选， 如果设为true或1， 则只删除⼀条， 默认false， 表示删除多条
> 
> # 删除_id为4的值
> db.stu.remove({"_id":4})
> # 删除所有age为43的数据
> db.stu.remove({"age":43})
> # 删除第一个age为43的数据
> db.stu.remove({"age":43},justOne:true)
> ```

# mongodb的高级查询

> ⽅法find()： 查询
>
> ```
> db.集合名称.find({条件⽂档}) 默认查询所有数据
> db.stu.find({age:18,name:黄忠})	查询age为18，name为黄忠的人
> ```
>
> ⽅法findOne()：查询，只返回第⼀个
>
> ```
> db.集合名称.findOne({条件⽂档})
> db.stu.findOne({age:18})
> ```
>
> ⽅法pretty()： 将结果格式化
>
> ```
> db.集合名称.find({条件⽂档}).pretty()
> db.stu.find({age:18}).pretty()
> # 格式化输出如下
> {
>         "_id" : 2,
>         "name" : "黄蓉",
>         "hometown" : "桃花岛",
>         "age" : 18,
>         "gender" : false
> }
> # 未格式化输出
> { "_id" : 2, "name" : "黄蓉", "hometown" : "桃花岛", "age" : 18, "gender" : false }
> ```



### 比较运算符

| 符号 | 含义     | 例子                         | 例子释义                     |
| ---- | -------- | ---------------------------- | ---------------------------- |
| $lt  | ⼩于     | db.stu.find({age:{$lt:18}})  | 查询年龄小于18的所有学生     |
| $lte | ⼩于等于 | db.stu.find({age:{$lte:18}}) | 查询年龄小于等于18的所有学生 |
| $gt  | ⼤于     | db.stu.find({age:{$gt:18}})  | 查询年龄大于18的所有学生     |
| $gte | ⼤于等于 | db.stu.find({age:{$gte:18}}) | 查询年龄大于等于18的所有学生 |
| $ne  | 不等于   | db.stu.find({age:{$ne:18}})  | 查询年龄不等于18的所有学生   |

### 逻辑运算符

> and：在json中写多个条件即可
>
> ```
> 查询年龄⼤于或等于18， 并且性别为true的学⽣
> db.stu.find({age:{$gte:18},gender:true})
> ```
>
> or:使⽤$or， 值为数组， 数组中每个元素为json
>
> ```
> 查询年龄⼤于18， 或性别为false的学⽣
> db.stu.find({$or:[{age:{$gt:18}},{gender:false}]})
> 
> 查询年龄⼤于18或性别为男⽣， 并且姓名是郭靖
> db.stu.find({$or:[{age:{$gte:18}},{gender:true}],name:'gj'})
> ```

### 范围运算符

> 使用$in，$nin 判断是否在某个范围内
> 例6：查询年龄为18、28的学生
>
> ~~~
> db.stu.find({age:{$in:[18,28]}})
> ~~~

### 支持正则表达式

> - 使用`/ /`或`$regex`编写正则表达式
> - 例7：查询姓黄的学生
>
> ~~~
> db.stu.find({name:/^黄/})
> db.stu.find({name:{$regex:'^黄'}}})
> 
> ~~~

### 自定义查询

> - 使用`$where`后面写一个函数，返回满足条件的数据
> - 例7：查询年龄大于30的学生
>
> ~~~
> db.stu.find({$where : function(){return this.age>20}})
> ~~~
>
> 

### Limit与skip

> **方法limit()：**用于读取指定数量的文档
>
> ~~~
> db.集合名称.find().limit(NUMBER)
> db.stu.find().limit(2)		查询两条学生信息
> 参数NUMBER表示要获取文档的条数
> 如果没有指定参数则显示集合中的所有文档
> ~~~
>
> **方法skip()：**用于跳过指定数量的文档
>
> ~~~
> db.集合名称.find().skip(NUMBER)
> db.stu.find().skip(2)	从第3条开始查询
> 参数NUMBER表示跳过的记录条数，默认值为0
> 例2：查询从第3条开始的学生信息
> ~~~
>
> **limit和skip一起使用**
>
> ~~~
> 方法limit()和skip()可以一起使用，不分先后顺序
> db.nums.find().limit(4).skip(5)
> 或
> db.nums.find().skip(5).limit(4)
> ~~~

### 投影

> **db.stu.find({},{name:1, gender:true})**
>
> ~~~
> { "_id" : 1, "name" : "郭靖", "gender" : true }
> { "_id" : 2, "name" : "黄蓉", "gender" : false }
> { "_id" : 3, "name" : "华筝", "gender" : false }
> { "_id" : 4, "name" : "黄药师", "gender" : true }
> { "_id" : 5, "name" : "段誉", "gender" : true }
> { "_id" : 6, "name" : "段王爷", "gender" : true }
> ~~~
>
> **db.stu.find({},{_id:0,name:1, gender:true})** 
>
> ~~~json
> # 设置_id：0 则不显示id
> { "name" : "郭靖", "gender" : true }
> { "name" : "黄蓉", "gender" : false }
> { "name" : "华筝", "gender" : false }
> { "name" : "黄药师", "gender" : true }
> { "name" : "段誉", "gender" : true }
> { "name" : "段王爷", "gender" : true }
> ~~~
>
> db.集合名称.find({},{字段名称:1,...})
>
> 返回指定字段，

### 排序

> **方法sort()**，用于对结果集进行排序
>
> ~~~
> db.集合名称.find().sort({字段:1,...})
> ~~~
>
> - 参数1为升序排列
> - 参数-1为降序排列
>
> ~~~
> 例1：根据性别降序，再根据年龄升序
> db.stu.find().sort({gender:-1,age:1})
> ~~~

### 统计个数

> **方法count()**用于统计结果集中文档条数
>
> ~~~
> db.集合名称.find({条件}).count()
> 也可以与为
> db.集合名称.count({条件})
> ~~~
>
> **例子**
>
> ~~~
> 例1：统计男生人数
> db.stu.find({gender:1}).count()
> 例2：统计年龄大于20的男性人数
> db.stu.count({age:{$gt:20},gender:true})
> ~~~

### 消除重复

> db.集合名称.distinct('去重字段',{条件})
>
> ~~~
> 例:查找年龄大于18的学生，来自哪些省份
> db.stu.distinct('hometown',{age:{$gt:18}})
> ~~~

# 聚合 

> ~~~
> db.集合名称.aggregate([ {管道 : {表达式}} ])
> ~~~

### 管道

| 方法     | 功能                                                       |      |
| -------- | ---------------------------------------------------------- | ---- |
| group$   | 将集合中的文档分组，可用于统计结果                         |      |
| $match   | 过滤数据，只输出符合条件的文档                             |      |
| $project | 修改输入文档的结构，如重命名、增加、删除字段、创建计算结果 |      |
| $sort    | 将输入文档排序后输出                                       |      |
| $limit   | 限制聚合管道返回的文档数                                   |      |
| $skip    | 跳过指定数量的文档，并返回余下的文档                       |      |
| $unwind  | 将数组类型的字段进行拆分                                   |      |

### 表达式

| 方法   | 功能                                   |      |
| ------ | -------------------------------------- | ---- |
| $sum   | 计算总和，$sum:1同count表示计数        |      |
| $avg   | 计算平均值                             |      |
| $min   | 获取最小值                             |      |
| $max   | 获取最大值                             |      |
| $push  | 在结果文档中插入值到一个数组中         |      |
| $first | 根据资源文档的排序获取第一个文档数据   |      |
| $last  | 根据资源文档的排序获取最后一个文档数据 |      |

### $group

- 将集合中的文档分组，可用于统计结果
- _id表示分组的依据，使用某个字段的格式为'$字段'
- 例1：统计男生、女生的总人数

```
db.stu.aggregate([
    {$group:
        {
            _id:'$gender',
            counter:{$sum:1}
        }
    }
])

结果
{ "_id" : false, "counter" : 2 }
{ "_id" : true, "counter" : 4 }
```

**Group by null**

- 将集合中所有文档分为一组
- 例2：求学生总人数、平均年龄

```
db.stu.aggregate([
    {$group:
        {
            _id:null,
            counter:{$sum:1},
            avgAge:{$avg:'$age'}
        }
    }
])

# 结果
{ "_id" : null, "counter" : 6, "avgAge" : 26.166666666666668 }

```

**透视数据**

- 例3：统计学生性别及学生姓名

```
db.stu.aggregate([
    {$group:
        {
            _id:'$gender',
            name:{$push:'$name'}
        }
    }
])

# 结果
{ "_id" : false, "name" : [ "黄蓉", "华筝" ] }
{ "_id" : true, "name" : [ "郭靖", "黄药师", "段誉", "段王爷" ] }
```

- 使用$$ROOT可以将文档内容加入到结果集的数组中，代码如下

```
db.stu.aggregate([
    {$group:
        {
            _id:'$gender',
            name:{$push:'$$ROOT'}
        }
    }
])

#结果
{ "_id" : false, "name" : [ { "_id" : 2, "name" : "黄蓉", "hometown" : "桃花岛", "age" : 18, "gender" : false }, { "_id" : 3, "name" : "华筝", "hometown" : "蒙古", "age" : 18, "gender" : false } ] }
{ "_id" : true, "name" : [ { "_id" : 1, "name" : "郭靖", "hometown" : "蒙古", "age" : 20, "gender" : true }, { "_id" : 4, "name" : "黄药师", "hometown" : "桃花岛", "age" : 40, "gender" : true }, { "_id" : 5, "name" : "段誉", "hometown" : "大理", "age" : 16, "gender" : true }, { "_id" : 6, "name" : "段王爷", "hometown" : "大理", "age" : 45, "gender" : true } ] }
```

### $match

- 用于过滤数据，只输出符合条件的文档
- 使用MongoDB的标准查询操作
- 例1：查询年龄大于20的学生

```
db.stu.aggregate([
    {$match:{age:{$gt:20}}}
])

# 结果
{ "_id" : 4, "name" : "黄药师", "hometown" : "桃花岛", "age" : 40, "gender" : true }
{ "_id" : 6, "name" : "段王爷", "hometown" : "大理", "age" : 45, "gender" : true }
```

- 例2：查询年龄大于20的男生、女生人数

```
db.stu.aggregate([
    {$match:{age:{$gt:20}}},
    {$group:{_id:'$gender',counter:{$sum:1}}}
])

# 结果
{ "_id" : true, "counter" : 2 }
```

### $project

- 修改输入文档的结构，如重命名、增加、删除字段、创建计算结果
- 例1：查询学生的姓名、年龄

```
db.stu.aggregate([
    {$project:{_id:0,name:1,age:1}}
])
# 结果
{ "name" : "郭靖", "age" : 20 }
{ "name" : "黄蓉", "age" : 18 }
{ "name" : "华筝", "age" : 18 }
{ "name" : "黄药师", "age" : 40 }
{ "name" : "段誉", "age" : 16 }
{ "name" : "段王爷", "age" : 45 }
```

- 例2：查询男生、女生人数，输出人数

```
db.stu.aggregate([
    {$group:{_id:'$gender',counter:{$sum:1}}},
    {$project:{_id:0,counter:1}}
])

# 结果
{ "counter" : 2 }
{ "counter" : 4 }
```

### $sort

- 将输入文档排序后输出
- 例1：查询学生信息，按年龄升序

```
db.stu.aggregate([{$sort:{age:1}}])

# 结果
{ "_id" : 5, "name" : "段誉", "hometown" : "大理", "age" : 16, "gender" : true }
{ "_id" : 2, "name" : "黄蓉", "hometown" : "桃花岛", "age" : 18, "gender" : false }
{ "_id" : 3, "name" : "华筝", "hometown" : "蒙古", "age" : 18, "gender" : false }
{ "_id" : 1, "name" : "郭靖", "hometown" : "蒙古", "age" : 20, "gender" : true }
{ "_id" : 4, "name" : "黄药师", "hometown" : "桃花岛", "age" : 40, "gender" : true }
{ "_id" : 6, "name" : "段王爷", "hometown" : "大理", "age" : 45, "gender" : true }
```

- 例2：查询男生、女生人数，按人数降序

```
db.stu.aggregate([
    {$group:{_id:'$gender',counter:{$sum:1}}},
    {$sort:{counter:-1}}
])

# 结果
{ "_id" : true, "counter" : 4 }
{ "_id" : false, "counter" : 2 }
```

### $limit与$skip

**$limit**

- 限制聚合管道返回的文档数
- 例1：查询2条学生信息

```
db.stu.aggregate([{$limit:2}])
# 结果
{ "_id" : 1, "name" : "郭靖", "hometown" : "蒙古", "age" : 20, "gender" : true }
{ "_id" : 2, "name" : "黄蓉", "hometown" : "桃花岛", "age" : 18, "gender" : false }
```

**$skip**

- 跳过指定数量的文档，并返回余下的文档
- 例2：查询从第3条开始的学生信息

```
db.stu.aggregate([{$skip:2}])

#结果 

{ "_id" : 3, "name" : "华筝", "hometown" : "蒙古", "age" : 18, "gender" : false }
{ "_id" : 4, "name" : "黄药师", "hometown" : "桃花岛", "age" : 40, "gender" : true }
{ "_id" : 5, "name" : "段誉", "hometown" : "大理", "age" : 16, "gender" : true }
{ "_id" : 6, "name" : "段王爷", "hometown" : "大理", "age" : 45, "gender" : true }
```

- 例3：统计男生、女生人数，按人数升序，取第二条数据

```
db.stu.aggregate([
    {$group:{_id:'$gender',counter:{$sum:1}}},
    {$sort:{counter:1}},
    {$skip:1},
    {$limit:1}
])
# 结果
{ "_id" : true, "counter" : 4 }
```

- 注意顺序：先写skip，再写limit

### $unwind

- 将文档中的某一个数组类型字段拆分成多条，每条包含数组中的一个值

**语法1**

- 对某字段值进行拆分

```
db.集合名称.aggregate([{$unwind:'$字段名称'}])
```

- 构造数据

```
db.t2.insert({_id:1,item:'t-shirt',size:['S','M','L']})
```

- 查询

```
db.t2.aggregate([{$unwind:'$size'}])
```

**语法2**

- 对某字段值进行拆分
- 处理空数组、非数组、无字段、null情况

```
db.inventory.aggregate([{
    $unwind:{
        path:'$字段名称',
        preserveNullAndEmptyArrays:<boolean>#防止数据丢失
    }
}])
```

- 构造数据

```
db.t3.insert([
{ "_id" : 1, "item" : "a", "size": [ "S", "M", "L"] },
{ "_id" : 2, "item" : "b", "size" : [ ] },
{ "_id" : 3, "item" : "c", "size": "M" },
{ "_id" : 4, "item" : "d" },
{ "_id" : 5, "item" : "e", "size" : null }
])
```

- 使用语法1查询

```
db.t3.aggregate([{$unwind:'$size'}])
```

- 查看查询结果，发现对于空数组、无字段、null的文档，都被丢弃了
- 问：如何能不丢弃呢？
- 答：使用语法2查询

```
db.t3.aggregate([{$unwind:{path:'$sizes',preserveNullAndEmptyArrays:true}}])
```

### 索引

- 在mysql中已经学习了索引，并知道索引对于查询速度的提升
- mongodb也支持索引，以提升查询速度

**步骤一：创建大量数据**

- 执行如下代码，向集合中插入10万条文档

```python
for(i=0;i<100000;i++){
    db.t1.insert({name:'test'+i,age:i})
}
```

**步骤二：数据查找性能分析**

- 查找姓名为'test10000'的文档

```python
db.t1.find({name:'test10000'})
```

- 使用explain()命令进行查询性能分析

```python
db.t1.find({name:'test10000'}).explain('executionStats')
```

- 其中的executionStats下的executionTimeMillis表示整体查询时间，单位是毫秒
- 性能分析结果如下图

![index2](%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87/index2.png)

**步骤三：建立索引**

- 创建索引
- 1表示升序，-1表示降序

```python
db.集合.ensureIndex({属性:1})
如
db.t1.ensureIndex({name:1})
```

**步骤四：对索引属性查询**

- 执行上面的同样的查询，并进行查询性能分析

```python
db.t1.find({name:'test10000'}).explain('executionStats')
```

- 性能分析结果如下图

#### ![index2](%E9%85%8D%E7%BD%AE%E5%9B%BE%E7%89%87/index2.png)

- 建立唯一索引，实现唯一约束的功能

```python
db.t1.ensureIndex({"name":1},{"unique":true})
```

- 联合索引，对多个属性建立一个索引，按照find()出现的顺序

```python
db.t1.ensureIndex({name:1,age:1})
```

- 查看文档所有索引

```python
db.t1.getIndexes()
```

- 删除索引

```python
db.t1.dropIndexes('索引名称')
```

# 与python交互

- 点击查看[官方文档](http://api.mongodb.org/python/current/tutorial.html)
- 安装python包

```c
sudo pip install pymongo
```

- 引入包pymongo

```
from pymongo import *
```

**MongoClient对象**

- 使用init方法创建连接对象

```c
client = MongoClient('主机ip',端口)
```

**Database对象**

- 通过client对象获取获得数据库对象

```c
db = client.数据库名称
```

**Collections对象**

- 通过db对象获取集合对象

```c
collections = db.集合名称
```

- 主要方法如下
  - insert_one：加入一条文档对象
  - insert_many：加入多条文档对象
  - find_one：查找一条文档对象
  - find：查找多条文档对象
  - update_one：更新一条文档对象
  - update_many：更新多条文档对象
  - delete_one：删除一条文档对象
  - delete_many：删除多条文档对象

**Cursor对象**

- 当调用集合对象的find()方法时，会返回Cursor对象
- 结合for...in...遍历cursor对象

### 增加

- 创建mongodb_insert1.py文件，增加一条文档对象

```python
#coding=utf-8

from pymongo import *

try:
    # 接收输入
    name=raw_input('请输入姓名：')
    home=raw_input('请输入家乡：')
    # 构造json对象
    doc={'name':name,'home':home}
    #调用mongo对象，完成insert
    client=MongoClient('localhost',27017)
    db=client.py3
    db.stu.insert_one(doc)
    print 'ok'
except Exception,e:
    print e
```

- 创建mongodb_insert2.py文件，增加多条文档对象

```python
#coding=utf-8

from pymongo import *

try:
    # 构造json对象
    doc1={'name':'hr','home':'thd'}
    doc2={'name':'mnc','home':'njc'}
    doc=[doc1,doc2]
    #调用mongo对象，完成insert
    client=MongoClient('localhost',27017)
    db=client.py3
    db.stu.insert_many(doc)
    print 'ok'
except Exception,e:
    print e
```

### 查询

- 创建mongodb_find1.py文件，查询一条文档对象

```
#coding=utf-8

from pymongo import *

try:
    client=MongoClient('localhost',27017)
    db=client.py3
    doc=db.stu.find_one()
    print '%s--%s'%(doc['name'],doc['hometown'])
except Exception,e:
    print e
```

- 创建mongodb_find2.py文件，查询多条文档对象

```
#coding=utf-8

from pymongo import *

try:
    client=MongoClient('localhost',27017)
    db=client.py3
    cursor=db.stu.find({'hometown':'大理'})
    for doc in cursor:
        print '%s--%s'%(doc['name'],doc['hometown'])
except Exception,e:
    print e
```

### 修改

- 创建mongodb_update1.py文件，修改一条文档对象

```
#coding=utf-8

from pymongo import *

try:
    client=MongoClient('localhost',27017)
    db=client.py3
    db.stu.update_one({'gender':False},{'$set':{'name':'hehe'}})
    print 'ok'
except Exception,e:
    print e
```

- 创建mongodb_update2.py文件，修改多条文档对象

```
#coding=utf-8

from pymongo import *

try:
    client=MongoClient('localhost',27017)
    db=client.py3
    db.stu.update_many({'gender':True},{'$set':{'name':'haha'}})
    print 'ok'
except Exception,e:
    print e
```

### 删除

- 创建mongodb_delete1.py文件，删除一条文档对象

```
#coding=utf-8

from pymongo import *

try:
    client=MongoClient('localhost',27017)
    db=client.py3
    db.stu.delete_one({'gender':True})
    print 'ok'
except Exception,e:
    print e
```

- 创建mongodb_delete2.py文件，删除多条文档对象

```
#coding=utf-8

from pymongo import *

try:
    client=MongoClient('localhost',27017)
    db=client.py3
    db.stu.delete_many({'gender':False})
    print 'ok'
except Exception,e:
    print e
```