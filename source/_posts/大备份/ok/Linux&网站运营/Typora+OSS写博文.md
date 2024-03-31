### 1 markdown编辑器Typora安装

略

### 2 OSS图床配置

云存储不是必须的，担心费用可以使用自建云存储，比如MinIO。

OSS是阿里云的对象存储服务，最低费用9￥/年，腾讯云、华为云等都有对应的对象存储服务，可以根据自己喜好选择，配置方法基本一致。

注意：一定！一定！要注意保护好唯一标识！桶名、服务器IP、CDN域名、accessKeyId、accessKeySecret！

#### 1 创建bucket

功能按需开通

没使用CDN的时候，需要匿名访问，把权限设置为公共读

后面使用CDN访问，就把权限设置为私有读写

![屏幕截图_20240321_144337](https://blog-resources.this0.com/image/202403232049206.png?x-oss-process=style/this0-blog)

#### 2 创建子用户

点右上角头像——AccessKey管理——开始使用子用户AccessKey

一定不要嫌麻烦而用主账号的AccessKey！

![屏幕截图_20240320_215650](https://blog-resources.this0.com/image/202403232049207.png?x-oss-process=style/this0-blog)

注意！点击确定之后出现的accessKeySecret只会在这时候出现一次，务必保存下来，之后无法获取！

#### 3 给子用户OSS权限

点击添加权限

![屏幕截图_20240320_220607](https://blog-resources.this0.com/image/202403232049208.png?x-oss-process=style/this0-blog)

选择OSS权限

![屏幕截图_20240320_220926](https://blog-resources.this0.com/image/202403232049210.png?x-oss-process=style/this0-blog)

#### 4 获取AccessKey和AccessKeySecret

记录下创建用户时创建的AccessKey和AccessKeySecret，AccessKey还能在这个页面获取，AccessKeySecret如上文所说，无法再获取

![屏幕截图_20240320_221452](https://blog-resources.this0.com/image/202403232049211.png?x-oss-process=style/this0-blog)

#### 5 OSS安全配置

###### 1 OSS跨域设置

![屏幕截图_20240321_152502](https://blog-resources.this0.com/image/202403232049212.png?x-oss-process=style/this0-blog)

###### 2 OSS开启防盗链

![屏幕截图_20240321_153100](https://blog-resources.this0.com/image/202403232049213.png?x-oss-process=style/this0-blog)



### 3 上传工具PicGo/PicList配置

上传工具根据自己喜好选择PicGo或者PicList，大同小异

笔者喜欢用PicList,在[PicList官网](https://piclist.cn/)下载安装

PicList内置了[配置教程](https://piclist.cn/manage#%E9%98%BF%E9%87%8C%E4%BA%91oss)，不再赘述

CDN访问需要在PliList里面配置一下CDN加速域名，不要上传完转义成存储桶名称访问，Typora的图像配置按需配置

![屏幕截图_20240321_144640](https://blog-resources.this0.com/image/202403232049214.png?x-oss-process=style/this0-blog)

至此，完成。

### 4 加速访问—CDN

使用云存储有被攻击的风险

云存储不是必须的，CDN同样也不是必须的，CDN需要不少的费用，没配置好安全措施，最少会被刷数以千计的￥！

#### 1 CDN 基础配置

##### 1 添加CDN域名

![屏幕截图_20240321_170519](https://blog-resources.this0.com/image/202403232049215.png?x-oss-process=style/this0-blog)

##### 2 配置源站域名

自动获取OSS桶的域名

![屏幕截图_20240321_170903](https://blog-resources.this0.com/image/202403232049216.png?x-oss-process=style/this0-blog)

确定，点击一键配置

![屏幕截图_20240321_162639](https://blog-resources.this0.com/image/202403232049217.png?x-oss-process=style/this0-blog)

跳转到CNAME信息页面

##### 3 配置CNANE

![屏幕截图_20240321_162707](https://blog-resources.this0.com/image/202403232049218.png?x-oss-process=style/this0-blog)

到此，已经可以使用CDN加速了

#### 2 CDN 证书配置

如图，证书可以申请免费证书，现在只有3个月免费证书了，到期无法续费，只能重新创建，看来需要另谋出路

![屏幕截图_20240321_172620](https://blog-resources.this0.com/image/202403232049219.png?x-oss-process=style/this0-blog)

#### 3 CDN其他配置

其他还可以开启流量限制，预防被刷流量，以及开启CDN的防盗链，以及性能优化

至此，完成！