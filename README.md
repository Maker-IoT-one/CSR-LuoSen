# 华为云命令下发
为了实现前端点击调用硬件的效果，可以使用华为云IOT命令下发平台进行是实现。

在官网上可以看到命令下发的实例只有Java实例，因此先用Java尝试一下

[华为云命令下发示例官方链接](https://support.huaweicloud.com/intl/zh-cn/usermanual-iothub/iot_01_0343.html)
## 导包
根据文档先导入maven依赖：通俗来讲就是导包，只是用maven工具罢了
``` java
<dependency>
    <groupId>com.huaweicloud.sdk</groupId>
    <artifactId>huaweicloud-sdk-core</artifactId>
    <version>[3.0.40-rc, 3.2.0)</version>
</dependency>
<dependency>
    <groupId>com.huaweicloud.sdk</groupId>
    <artifactId>huaweicloud-sdk-iotda</artifactId>
    <version>[3.0.40-rc, 3.2.0)</version>
</dependency>
```
## 进行命令下发命令书写
官方链接可以看到具体代码，但是里面缺少一行参数，我对此进行修改
[可点击查看修改代码](./华为云/命令下发/java命令下发.txt)
简单讲解一下代码：
- 先找到你华为云所在地区进行填写——认证
- 然后通过华为云账号获取AK和SK验证你的身份——认证
- 项目id——认证
- 设备id——请求参数
- 参数id——请求参数
- 发送的键值对，如文档中对应的就是Light:ON 打开灯——请求参数
## python版本
官网没有python的完整示例，但是有[部分的展示](https://support.huaweicloud.com/intl/zh-cn/sdkreference-iothub/iot_10_10003.html)，我根据java代码进行修改

[python版命令下发](./华为云/命令下发/python华为云命令下发.txt)

