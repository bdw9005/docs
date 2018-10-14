# 媒体服务
火信的消息分为普通消息和媒体消息。媒体消息一般比较大，发送时需要先上传媒体文件到媒体服务器，得到一个url地址，然后再把包含这个url地址的消息发出去。火信同时支持内置媒体服务器和七牛媒体服务器。客户端不用修改。

#### 使用内置媒体服务器。
修改如下配置，```media.server.use_qiniu```配置为0，```local.media.server.ip```配置成本机的公网ip。这样所有媒体文件都讲上传到fs目录，按照日期和类型存放。
```
media.server.use_qiniu 0
local.media.server.ip 0.0.0.0
local.media.storage.root fs
```
> 内置文件服务器有很大的限制。受限于linux的inode数，文件不能太多，需要定时清理，另外媒体文件提交较大，没有cdn加速下载会很慢，建议使用七牛媒体服务器，价格不贵，而且有CDN加速。

#### 七牛服务器
修改如下配置，```media.server.use_qiniu```配置为1，其它配置都需要配置正确。
```
media.server.use_qiniu 1
qiniu.server_url  http://up.qbox.me
qiniu.bucket_name media
qiniu.bucket_domain http://pghnpyzos.bkt.clouddn.com
qiniu.access_key tU3vdBK5BL5j4N7jI5N5uZgq_HQDo170w5C9Amnn
qiniu.secret_key YfQIJdgp5YGhwEw14vGpaD2HJZsuJldWtqens7i5
```

#### 使用其它服务器
上述两种服务服务器的url中都带有32位的uuid，基本上不会被穷举。但生成的url没有访问控制，传输过程中也没有加密，因此如果客户需要传输非常敏感的媒体文件，请在客户端上传文件到自己的应用服务器，然后再调用sdk发送消息。