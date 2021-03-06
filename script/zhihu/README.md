# 知乎助手

## 介绍

去除知乎广告，提供付费内容提醒、黑名单增强等优化阅读体验的功能。

分为Plus和Lite两个版本，Lite只提供最纯粹的去广告功能，Plus带有一些优化阅读体验的功能。

目前已实现(带✨的为Plus版本的功能)：

1. 去除知乎开屏的广告
2. 去除关注列表的广告
3. 去除推荐列表的广告
4. 去除回答列表的广告
5. 去除回答列表的圆桌
6. 去除会员页面弹出广告
7. 去除知乎直播红点
8. 去除知乎指南提示
9. 付费内容文首提醒✨
10. 去除推荐列表的付费推荐内容✨
11. 去除官方账号的推广消息✨
12. 去除推荐列表中黑名单用户的回答✨
13. 去除回答列表中黑名单用户的回答✨
14. 去除关注顶部的最常访问✨
15. 去除未读消息的红点✨

## 最近更新

1. 付费内容文首提醒
2. 会员页面弹出广告屏蔽
3. 去除脚本内置的答主黑名单，现在屏蔽谁完全由你决定。如果出现屏蔽失效，请重新获取黑名单。
4. 解决关注列表切换为”时间排序“时去广告失效的问题
5. 解决知乎直播无法访问的问题
6. 去除回答列表的圆桌

## 去广告

如果出现去广告无效的情况，通常有两种可能：一种是CDN服务器的IP没有加到MITM中引起的；另外一种是你有其他的规则优先级更高，知乎去广告的规则覆盖掉了。

建议解决方法：

1. **将知乎去广告规则的优先级调整到最高**
2. 重启知乎
3. 清理知乎的缓存
4. 卸载知乎后重装
5. 安装已经验证过的版本
6. [点击这里反馈给我](https://github.com/blackmatrix7/ios_rule_script/issues/new)

### 验证情况

2020年8月22日：

在知乎 V6.52.0(2548)、Surge4.10.0(1800) TF、Quantumult X 1.0.14(367) TF、Loon 2.1.3(197) TF 中验证通过。

2020年8月8日：

在知乎 V6.51.1(2518)、Surge4.10.0(1788) TF、Quantumult X 1.0.14(359) TF、Loon 2.1.3(191) TF 中验证通过。

> 部分去广告的思路来自 [@onewayticket255](https://github.com/onewayticket255/Surge-Script)

## 付费内容提醒

遇到需要付费阅读的回答时，会**将付费内容的提醒置顶**。避免阅读中途发现内容需要付费的情况，提高阅读体验。

效果如下图：

![](https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/script/zhihu/images/04.jpg)

## 黑名单增强

知乎的黑名单设计，无法屏蔽黑名单用户的公开信息。将某人拉黑后，他的回答依旧会出现在推荐列表和回答列表中。

黑名单增强就是对黑名单用户的回答进行屏蔽，让他的回答从推荐列表和回答列表中消失。

如果需要定向查看某个黑名单的用户，请搜索他的名称，然后点进去看他的回答。

#### 自定义黑名单

**首次使用时，需要获取一次完整的黑名单**。请从“我的”-“设置”-“屏蔽设置”-“管理黑名单”，进入黑名单列表。不断往下滑动，直到滑动到列表底部。滑动到底部后，会弹出通知“获取脚本黑名单结束”，表示黑名单获取完成。此时黑名单为脚本内置黑名单与用户自定义黑名单的并集，如果不需要脚本内置的黑名单，则fork后自行修改。

黑名单匹配方式为用户名，同名用户都会被屏蔽，“[已重置]”除外。

每次添加或移除黑名单用户，脚本内置的黑名单也会同步更新。

![](https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/script/zhihu/images/01.jpg)

![](https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/script/zhihu/images/03.jpg)

![](https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/script/zhihu/images/02.jpg)

## 配置说明(Plus)

#### Surge

使用模块

```ini
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/script/zhihu/zhihu_plus.sgmodule
```

#### Loon

配置文件

```ini
[Rule]
# 知乎去广告
DOMAIN,118.89.204.198,REJECT,no-resolve
DOMAIN-SUFFIX,118.89.204.198,REJECT,no-resolve
DOMAIN-KEYWORD,118.89.204.198,REJECT,no-resolve
IP-CIDR,118.89.204.198/32,REJECT,no-resolve
DOMAIN,appcloud2.zhihu.com,REJECT
DOMAIN,appcloud2.in.zhihu.com,REJECT
USER-AGENT,AVOS*,REJECT
URL-REGEX,^https?:\/\/api\.zhihu\.com\/(me\/guides|drama\/living-info|ad|fringe|commercial|market\/popovers|search\/(top|preset|tab)|.*featured-comment-ad),REJECT

[URL Rewrite]
# 知乎去除最常访问
^https?:\/\/api\.zhihu\.com\/moments\/recent url reject-dict
# 知乎去除未读消息红点
^https?:\/\/api\.zhihu\.com\/notifications\/v3\/count url reject-dict

[Remote Script]
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/script/zhihu/zhihu_plus.loon, tag=知乎助手_去广告及黑名单增强, enabled=true
```

Loon 2.1.3(193) TF + 可以使用插件Plugin

```ini
[Plugin]
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/script/zhihu/zhihu_plus.loonplugin, tag=知乎助手_去广告及体验增强, enabled=true
```

#### Quantumult X

配置文件

```ini
[filter_local]
# 知乎去广告
DOMAIN,118.89.204.198,REJECT
DOMAIN-SUFFIX,118.89.204.198,REJECT
DOMAIN-KEYWORD,118.89.204.198,REJECT
IP-CIDR,118.89.204.198/32,REJECT
USER-AGENT,AVOS*,REJECT
DOMAIN-SUFFIX,appcloud2.zhihu.com,REJECT
DOMAIN-SUFFIX,appcloud2.in.zhihu.com,REJECT

[rewrite_remote]
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/script/zhihu/zhihu_plus.quanx, tag=知乎助手_去广告及体验增强, update-interval=86400, opt-parser=false, enabled=true
```

## 配置说明(Lite)

知乎去除最常访问、知乎去除未读消息红点、知乎指南屏蔽、黑名单功能增强已不属于屏蔽广告范畴，如果只想单纯去广告，使用下面的Lite版本的配置。

### Surge

使用模块

```ini
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/script/zhihu/zhihu_lite.sgmodule
```

#### Quantumult X

配置文件

```ini
[filter_local]
# 知乎去广告
DOMAIN,118.89.204.198,REJECT
DOMAIN-SUFFIX,118.89.204.198,REJECT
DOMAIN-KEYWORD,118.89.204.198,REJECT
IP-CIDR,118.89.204.198/32,REJECT
USER-AGENT,AVOS*,REJECT
DOMAIN-SUFFIX,appcloud2.zhihu.com,REJECT
DOMAIN-SUFFIX,appcloud2.in.zhihu.com,REJECT

[rewrite_remote]
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/script/zhihu/zhihu_lite.quanx, tag=知乎助手_去广告, update-interval=86400, opt-parser=false, enabled=true
```

### Loon

配置文件

```ini
[Rule]
# 知乎去广告
DOMAIN,118.89.204.198,REJECT,no-resolve
DOMAIN-SUFFIX,118.89.204.198,REJECT,no-resolve
DOMAIN-KEYWORD,118.89.204.198,REJECT,no-resolve
IP-CIDR,118.89.204.198/32,REJECT,no-resolve
DOMAIN,appcloud2.zhihu.com,REJECT
DOMAIN,appcloud2.in.zhihu.com,REJECT
USER-AGENT,AVOS*,REJECT
URL-REGEX,^https?:\/\/api\.zhihu\.com\/(me\/guides|drama\/living-info|ad|fringe|commercial|market\/popovers|search\/(top|preset|tab)|.*featured-comment-ad),REJECT

[Remote Script]
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/script/zhihu/zhihu_lite.loon, tag=知乎助手_去广告, enabled=true
```

Loon 2.1.3(193) TF + 可以使用插件Plugin

```ini
[Plugin]
https://raw.githubusercontent.com/blackmatrix7/ios_rule_script/master/script/zhihu/zhihu_lite.loonplugin, tag=知乎助手_去广告, enabled=true
```

## 其他问题

### 脚本内置黑名单

对于脚本内置的黑名单，**保持谨慎和克制的原则**，只加入无法通过加入黑名单进行屏蔽的账号。如需要屏蔽更多的账户，可以由使用者手动将其加入黑名单来实现。

推荐列表中脚本内置的黑名单基本上都已去除，只保留”会员推荐“等几个，因为这些都不是账号，不能通过加入黑名单来屏蔽。并且， 会员推荐的屏蔽功能，只有在你获取过一次黑名单后才会生效。如果你用的是Lite版本，完全不用担心屏蔽问题。

官方消息中脚本内置的黑名单会比较多，如知乎小伙伴、知乎视频、知乎亲子、知乎团队、知乎好物推荐、知乎盐选会员、知乎礼券、知乎校园等等，因为这些账号都没办法通过加入黑名单来屏蔽。

### 知乎直播无法访问

知乎去广告配置**不会导致知乎直播无法访问**。目前已知部分整合的去广告规则集合，会导致知乎直播无法访问。

如果出现知乎直播无法访问的情况，请开启抓包/调试/记录日志等功能，确认是哪条规则影响知乎直播的正常访问，将其删除或编写修正规则覆盖掉它。

如果没办法修改第三方的规则，提供几个修正方案。

#### Surge/Loon

Surge和Loon的知乎直播修正，提供两种方案。

一种是在本地规则中修改，覆盖掉远程引用的规则集，适用于远程规则集配置错误导致知乎直播无法访问的情况。

```ini
[Rule]
# 知乎直播修正
URL-REGEX,^https?:\/\/api\.zhihu\.com\/drama\/,DIRECT
```

一种是在url复写中进行修改，覆盖掉远程的订阅复写，适用于Loon远程订阅复写配置错误导致知乎直播无法访问的情况。

```ini
[URL Rewrite]
# 知乎直播修正
^https?:\/\/api\.zhihu\.com\/drama\/ https://api.zhihu.com/drama/ header
```

根据实际情况二选一即可。如果使用插件，为覆盖各种场景，两个规则都写入到插件中了。

如果远程错误的规则是在模块或插件中的，由于模块和插件优先级很高，上面的修正可能不会生效，建议联系插件作者修改。

#### Quantumult X

增加一条本地复写规则，覆盖掉远程的复写规则

```ini
# 知乎直播修正
^https?:\/\/api\.zhihu\.com\/drama url 307 https://api.zhihu.com/drama
```

### 想法不存在

拦截知乎APP获取CDN服务器地址，改为由api.zhihu.com获取数据时，点击想法的评论，有较大概率会返回”想法不存在“或”似乎出了点问题“，因为所有的功能都依赖于拦截知乎APP获取CDN服务器地址，暂时无解。

## 最后

维护不易，如果能帮上你，麻烦给个Star⭐。如果没能帮上你，麻烦[点击这里反馈给我](https://github.com/blackmatrix7/ios_rule_script/issues/new)，个人测试覆盖场景有限，你的及时反馈可以让我尽快排查和解决问题。

特别感谢：

[@onewayticket255](https://github.com/onewayticket255/Surge-Script)

[@fiiir](https://github.com/fiiir)