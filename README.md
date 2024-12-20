# GO mod

## go-db

* 地址: [go-db](https://github.com/clong1995/go-db)
* 说明:
* 用法:

```go

```

## go-config

* 地址: [go-config](https://github.com/clong1995/go-config)
* 说明:
* 用法:

```go

```

## go-id

### 地址: [go-sid](https://github.com/clong1995/go-sid)

### 说明:

1. 生成唯一ID:
   > 为什么是`int64`，而不是`uint64`?  
   > 常用的数据库`PostgreSQL`和`Oracle`都没有原生的`UNSIGNED`定义  
   > `MySQL`支持`UNSIGNED`,但某些驱动都会反射加转型处理  
   > 为了通用和高效,选择int64

   > ID的位结构:

   | 46 bits | 6 bits | 12 bits |
   |:-------:|:------:|:-------:|
   |   时间戳   |  机器ID  |   序列号   |
   > **时间戳**  
   > `epoch`的起点为1136185445000 _(utc 2006-01-02 15:04:05)_  
   >  46bits 可持续运行约2231年  _(大约将在 utc 4237-01-02 15:04:05 后无法使用)_  
   > **机器ID**  
   > 6bits 最大可表示64台设备,中小型项目IDC可控单位不建议超过64台设备  
   > 如果项目体量大于64台设备, 采用集中式ID生成服务是更好的选择  
   > **序列号**  
   > 12bits 毫秒内可生成4096个, 并发量足够
   ```
   func Generate() int64
   ```

2. 提取ID的时间戳、机器ID和序列号:
   ```
   func Extract(id int64) (timestamp int64, machineID int, sequence int64)
   ```

3. 直接生成特定时间和机器的ID:
   > 为什么要有这个方法?  
   > 在数据库中,id可能是主键或者至少有索引  
   > 雪花id中包含时间信息,可以直接拿id作为查询的时间条件,进行高效查询
   ```
   func Deterministic(timestamp int64) (int64, error) 
   ```

4. 编码:
   > 为什么要编码?
   > 1. 某些语言是没有int64的支持,某些硬件平台也没有64位支持
   > 2. 编码可以防止id被推测引起漏洞  
        > 默认编码是经过xor的,  
        > 同一个id每次都会编码位不同的字符串,但是能被解码为同一个id
   ```
   func Encode(num int64) string
   ```
5. 解码:
   ```
   func Decode(encoded string) (result int64)
   ```

## go-server

* 地址: [go-db](https://github.com/clong1995/go-server)
* 说明:
* 用法:

```go

```

## go-encipher

* 地址: [go-encipher](https://github.com/clong1995/go-encipher)
* 说明:
* 用法:

```go

```

## go-auth

### 地址: [go-encipher](https://github.com/clong1995/go-auth)

### 说明:

1. 提取数据中用户ak,校验数据签名:
   ```
   func Check(sign string, req []byte) (ak string, err error)
   ```
2. 通过sk进行数据签名:
   ```
   func Sign(req []byte, sk string) (sign string, err error)
   ```
3. 设置sk的生成逻辑, **保证这个算法不会被暴露**:
   > 为什么要在代码中设计sk的生成逻辑?  
   > 避免在还没保证数据完整性,就提前做数据库或者缓存的的查询
   ```
   func Encode(e func(session int64, id int64) int64)
   ```
4. 通过ak编码sk
   ```
   func SecretAccess(ak string) (secretAccessKey string, err error)
   ```
5. 编码ak
   ```
   func AccessID(id, session int64) (ak string, err error)
   ```
6. 获取id
   ```
   func AccessID(id, session int64) (ak string, err error)
   ```
