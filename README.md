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

## go-sid

* 地址: [go-sid](https://github.com/clong1995/go-sid)
* 说明:
* 用法:

```go

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

3. 设置sk的生成逻辑:  
   为什么要在代码中设计sk的生成逻辑?  
   避免在还没保证数据完整性,就提前做数据库或者缓存的的查询 
   ```
   func Encode(e func(session int64, id int64) int64)
   ```
