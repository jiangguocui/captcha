# captcha
# 验证码实现设计

验证码不是一个功能性的需求，他并不能带来业务的提升，也不能带来任何价值。验证码只是为了解决机器问题才诞生的。在设计和验证码演化的过程中，必须同时考虑安全性和体验。


### 设计要点


```
1、图片验证一次性
2、超时未验证失效
3、支持４位验证码，字符以英文和数字组成
4、支持简单干扰

```

### 存储选择

首先我想到了缓存。目前比较流行的缓存服务是redis。操作速度是传统关系型数据库查询的几十甚至上百倍，性能上面没问题。我们知道，验证码是有时效的，可以利用他的缓存时效性

实现

```
１、调接口生成一个验证码signature，和验证码图片数据。
	base64(timestamp:random:sign(timestamp+random+code+secretKey))
２、验证时，带上signature和newcode.
	(1)拿到signature中timestamp，判断验证码是否过期
	(2)判断sign(timestamp+random+code+secretKey）和sign(timestamp+random+newcode+secretKey)是否相等，不相等，可能是签名被篡改了或者验证码输入错误
	(3)判断该签名是否已在黑名单中，如果已在黑命名单说明已经被验证过了
　　　　　(4)验证通过，加入黑名单。即将该signature存入redis,有效期设置为5分钟

```





