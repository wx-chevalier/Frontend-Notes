# AppStore

## Reference

- [ipv6, 后台定位等审核问题的终极解决方案汇总](https://github.com/wg689/Solve-App-Store-Review-Problem)

# 证书

> [ iOS Provisioning Profile(Certificate) 与 Code Signing 详解 ](http://blog.csdn.net/phunxm/article/details/42685597/)

![](https://coding.net/u/hoteam/p/Cache/git/raw/master/2016/8/2/5B92CF5E-7B97-46C6-8CE6-1EDC12C8BF08.png)

AppStore 的证书与绑定机制对于笔者感觉还是蛮复杂的，不过流程多走即便可能就会清晰很多。首先需要明白的是，在苹果的证书体系中，Certificates 与 Identifiers 是相互独立的，笔者之前经常有个疑问就是最早提交某个 APP 审核的开发者的 P12 丢失了，那后续重新申请的证书还可以用于提交后续版本吗？答案肯定是可以的。一般来说，某个 Certificates+Identifiers 会得到一个 MobileProvision 文件，而该文件就是在进行本地开发、打包上传时所必须的。![](http://www.dcloud.io/docs/a/cert/idc_ioscert.png) iOS 有两种证书和描述文件：

| 证书类型                           | 使用场景          |
| ---------------------------------- | ----------------- |
| 开发 (Development) 证书和描述文件  | 用于开发测试      |
| 发布 (Distribution) 证书和描述文件 | 用于提交 Appstore |

## App ID & Bundle Identifier

在苹果官方的开发者计划(Apple Developer Member Center )层面，App ID 即 Product ID，用于标识一个或者一组 App。

在 Mac/iOS 开发语境中，**bundle**(捆绑) 是指一个内部结构按照标准规则组织的特殊目录。在 Mac OS 应用程序目录下的某个 \*.app 上可右键显示包内容(Contents )，其本质上就是可执行二进制文件(MacOS/ )及其资源(Resources/ )的[打包组合](http://blog.sina.com.cn/s/blog_7b9d64af0101jmj2.html)。因此，在 Xcode 中配置的 Bundle Identifier 必须和 App ID 是一致的(Explicit )或匹配的(Wildcard )。

App ID 字符串通常以**反域名**(reverse-domain-name )格式的 Company Identifier(Company ID )作为前缀(Prefix/Seed )，一般不超过 255 个 ASCII 字符。

App ID 全名会被追加 Application Identifier Prefix(一般为 TeamID.)，分为两类：

- Explicit App ID：唯一的 App ID，用于唯一标识一个应用程序。例如 “com.apple.garageband” 这个 App ID，用于标识 Bundle Identifier 为 “com.apple.garageband” 的 App。
- Wildcard App ID：含有通配符的 App ID，用于标识一组应用程序。例如 “_”(实际上是 Application Identifier Prefix)表示所有应用程序；而 “com.apple._” 可以表示 Bundle Identifier 以 “com.apple.” 开头(苹果公司)的所有应用程序。

用户可在 Developer Member Center 网站上注册(Register )或删除(Delete )已注册的 App IDs。App ID **被配置到**【XcodeTarget|Info|Bundle Identifier 】下；对于 Wildcard App ID，只要 bundle identifier 包含其作为 Prefix/Seed 即可。![](http://www.dcloud.io/docs/a/cert/id_name.png) ![](http://www.dcloud.io/docs/a/cert/id_id.png)

## Certificates

iOS 证书是用来证明 iOS App 内容(bundle with executable and resources )的合法性和完整性的**数字证书**。对于想安装到真机或发布到 AppStore 的应用程序(App )，只有经过**签名验证**(Signature Validated )才能确保来源可信，并且保证 App 内容是完整、未经篡改的。

iOS 证书分为两类：Development 和 Production(Distribution )。

- Development 证书用来开发和调试应用程序：A **development certificate** identifies you, as a team member, in a development provisioning profile that allows apps signed by you to **launch **on devices.
- Production 主要用来分发应用程序(根据证书种类有不同作用): A **\*distribution certificate\*** identifies your team or organization in a distribution provisioning profile and allows you to **\*submit \***your app to the store. Only a team agent or an admin can create a distribution certificate.

普通个人开发账号最多可注册 iOS Development/Distribution 证书各 2 个，用户可在网站上删除(Revoke )已注册的 Certificate。Apple 证书颁发机构 WWDRCA(Apple Worldwide Developer Relations Certification Authority) 将使用其 private key 对 CSR 中的 public key 和一些身份信息进行加密签名生成数字证书(ios_development.cer )并记录在案(Apple Member Center )。![](http://img.blog.csdn.net/20150412074512682) ![](http://img.blog.csdn.net/20150422073707077)

而 P12 文件，正是用于在多台设备间共享证书。在 Keychain Access|Certificates 中选中欲导出的 certificate 或其下 private key，右键 Export 或者通过菜单 File|Export Items 导出 Certificates.p12——PKCS12 file holds the private key and certificate。其 他 Mac 机器上双击 Certificates.p12(如有密码需输入密码)即可安装该共享证书。有了共享证书之后，在开发者网站上将欲调试的 iOS 设备注册到该开发者账号名下，并下载对应证书授权了 iOS 调试设备的 Provisioning Profile 文件，方可在 iOS 真机设备上开发调试。

## Provision Profiles

Provisioning Profile 文件包含了上述的所有内容：证书、App ID 和 设备 ID。![](http://img.blog.csdn.net/20150126225313444) 一个 Provisioning Profile 对应一个 Explicit App ID 或 Wildcard App ID(一组相同 Prefix/Seed 的 App IDs)。在网站上手动创建一个 Provisioning Profile 时，需要依次指定 App ID(单选)、证书(Certificates，可多选)和设备(Devices，可多选)。用户可在网站上删除(Delete )已注册的 Provisioning Profiles。Provisioning Profile 决定 Xcode 用哪个证书(公钥)/ 私钥组合(Key Pair/Signing Identity )来签署应用程序(Signing Product )，并将在应用程序打包时嵌入到 .ipa 包里。安装应用程序时，Provisioning Profile 文件被拷贝到 iOS 设备中，运行该 iOS App 的设备通过它来认证安装的程序。如果要打包到真机上运行一个 APP，一般要经历以下三步：

- 首先，需要指明它的 App ID，并且验证 Bundle ID 是否与其一致；
- 其次，需要证书对应的私钥来进行签名，用于标识这个 APP 是合法、安全、完整的；
- 然后，如果是真机调试，需要确认这台设备是否授权运行该 APP。Provisioning Profile 把这些信息全部打包在一起，方便我们在调试和发布程序打包时使用。这样，只要在不同的情况下选择不同的 Provisioning Profile 文件就可以了。Provisioning Profile 也分为 Development 和 Distribution 两类，有效期同 Certificate 一样。Distribution 版本的 ProvisioningProfile 主要用于提交 App Store 审核，其中不指定开发测试的 Devices(0，unlimited)。App ID 为 Wildcard App ID(\* )。App Store 审核通过上架后，允许所有 iOS 设备(Deployment Target )上安装运行该 App。Xcode 将全部供应配置文件(包括用户手动下载安装的和 Xcode 自动创建的 Team Provisioning Profile)放在目录 ~/Library/MobileDevice/Provisioning Profiles 下。

## Xcode 7 免证书调试

所谓 “ 免证书 ” 真机调试，并不是真的不需要证书，Xcode 真机调试原有的证书配置体系仍在 ——All iOS, tvOS, and watchOS appsmust be code signed and provisioned to launch on a device. 所以，上文啰嗦几千字还是有点用的。自 Xcode7 开始，原来基于付费开发者账号及自助生成证书及配置文件的繁琐过程被苹果简化，Xcode 将针对任何普通账号自动为联调真机生成所需相关的证书及配置文件。当你打算向 App Store 提交发布应用，才需要付费。第一步：进入 Xcode Preferences|Accounts，添加自己的 Apple ID 账号。第二步：Build Settings|Code Signing 下的 Provisioning Profile 选择 Automatic，Code Signing Identity 选择 Automatic 下的 iOS Developer。第三步：General 配置 Bundle identifier，Team 下拉选择苹果 Member Center 自动为你的账号生成的 Personal Team ID。自己的账号在调试公司或其他第三方 APP 代码时，若填写 Bundle identifier 为他人账号注册的 APP ID(例如苹果相机应用 com.apple.camera)，会报错: No provisioning profiles with a valid signing identity (i.e. certificate and private key pair) matching the bundle identifier “com.apple.camera” were found. 即使编译通过了，可能运行时 APP 自身与服务器校验也可能会报签名错误，肿么办？？？Her skill：此时，可以在他人原有 App ID 基础上添加后缀(例如 com.apple.camera.extension)，配置成应用的衍生插件(相当于置于同一 App Group 下)就可以快乐的玩耍了。如果启动 APP 时，Xcode 报错 “process launch failed: Security”或 iPhone 报错【不受信任的开发者】，此时需要到 iPhone 通用配置中的描述文件(最新系统中可能叫设备管理)中，在描述文件(开发商应用)中选择对应的描述文件(你的 Apple ID)点击 信任或验证 即可。

# 内容 / 功能 Issue

## UGC 社区审核

笔者之前做过体育社交类产品，提交的时候被拒了，原因是没有添加内容举报的功能。

# IPv6 Issue

> [针对苹果 iOS 最新审核要求为应用兼容 IPv6](http://www.jianshu.com/p/54b989098537)

自 2016 年 6 月 1 日起，苹果要求所有提交 App Store 的 iOS 应用必须支持 IPv6-only 环境，背景也是众所周知的，IPv4 地址已基本分配完毕，同时 IPv6 比 IPv4 也更加高效，向 IPv6 过渡是大势所趋。

## IPv6 Only

首先需要明确一点，在 App Store 审核 APP 的 IPv6-only 的环境下也是可以正常访问 IPv4 的服务的，只是首先由 DNS64 将解析出来的 IPv4 地址转成兼容的 IPv6 地址，然后访问 IPv4 服务时通过 NAT64 网关对 IPv4 和 IPv6 进行 NAT，并不需要客户有实际的 IPv6 服务。如下图所示: ![](http://upload-images.jianshu.io/upload_images/273968-856b69a836ade53a.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 客户端在向 DNS64 请求一个域名的 IPv6 地址时，DNS64 会向域名的授权 DNS 请求 IPv6 地址，如果存在 IPv6 地址，则直接给客户端返回 IPv6 地址，如果不存在 IPv6 地址，则向授权请求 IPv4 地址，并将返回的 IPv4 地址转换为兼容的 IPv6 地址。以 Google DNS64 为例说明转换规则，分别请求 dnspod.cn 的 A 记录(IPv4 地址)和 AAAA 记录(IPv6 地址): ![](http://upload-images.jianshu.io/upload_images/273968-b1d11427a1922f10.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 从解析结果可以看出 IPv4 地址对应的 IPv6 地址，后 32 位的 3b25:7465 实际上就是 IPv4 地址的 16 进制表示 59=0x3b，37=0x25，116=0x74，101=0x65，明白该规则后也可以自己进行 IPv4 向兼容的 IPv6 地址的转换，如 119.29.29.29 的兼容 IPv6 地址为 64:ff9b::771d:1d1d，其中::表示为全 0。DNS64 解析流程如下图所示: ![](http://upload-images.jianshu.io/upload_images/273968-9afe85bbacad3275.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

### 搭建 IPv6 测试环境

用 Mac 做一个热点，然后用 iPhone 连接这个 Wi-Fi，在 “System Preferences” 界面选中 “Sharing” 的同时，要按住 “Option” 键。![](http://upload-images.jianshu.io/upload_images/841355-bd2a25d779153e4c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 之后在 “Sharing” 界面中，我们会看到和之前不一样的地方，就是红框所标的地方，多了一个叫 “Create NAT64 Network” 的选框，选中它。![](http://upload-images.jianshu.io/upload_images/841355-8e5aa1eac3c24a8d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

## 开发修正

### 避免使用底层的网络 API

下图展示的蓝色部分的这些 API 都是不存在兼容性问题的，而我们平时自己用的包括那些第三方的网络库大部分都是用的这些 API。![](http://upload-images.jianshu.io/upload_images/1196725-d7c683dce4c7b70f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 大部分情况下，我们用高级的 API 完全能够实现我们的需求，而且高级 API 封装的很便于使用，很多底层的像适配 IPv6 的工作都已经帮我们做好了。而用底层 API 会有大量的工作要我们自己来做，更容易产生 bug。但你如果确实需要用底层的 POSIX socket API, 请参照这个 RFC4038: Application Aspects of IPv6 Transition 的指导。

### 避免直接使用 IP 地址

比如下面这个 API，nodename 这个参数不要传 IP 地址，而应该用域名 ![](http://upload-images.jianshu.io/upload_images/1196725-578f17a4a4ef0a69.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240) 这个方法在著名的第三方 Reachability 中是用到的，我们常用的第三方网络库 AFNetworking 就用了这个。所以用到的同学得好好查一下了。

### 检查不兼容的 IPv6 代码

```
inet_addr()

inet_aton()

inet_lnaof()

inet_makeaddr()

inet_netof()

inet_network()

inet_ntoa()

inet_ntoa_r()

bindresvport()

getipv4sourcefilter()

setipv4sourcefilter()
```

以及如下的一些数据类型 : ![](http://upload-images.jianshu.io/upload_images/841355-dd37905d642ca9bd.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://153.3.251.190:11900/AppStore)
