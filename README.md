# :cn: appleGameSVN(游戏城GameCitys)
## 开发环境
- 开发工具: vs2017
- 开发语言: c#
- 数据库:msql 5.7
- 涉及技术及框架:
  - asp.net mvc core
  - entityframework core,Pomelo.Entityfaramwork.MySql
  - bootstrap
  - jquery
  - html5
## 应用场景
>   玩家登陆微型客户端点击分享链接既可登陆游戏.登陆后进入游戏大厅,游戏大厅有若干游戏房间.玩家可以选择房间进入.也可以自己建立新的房间,建立房间时可以选择玩不同的游戏.建立房间的玩家成为房主,可以配置房间.游戏内容和人数限制根据选择游戏项目的不同而变化.系统有一个管理员.
## 游戏功能
 - [x] 微信
   - [x] 微信openId认证
   - [x] 微信支付
   - [x] 微信红包
   - [x] 微信分享
- [x]  游戏大厅
    - [x] 自动加入房间
    - [x] 新建房间
    - [x] 随机加入房间
- [x]  房间 
  - [x]  房间内即时文字聊天
  - [ ]  房间内语音短消息聊天
- [x]  玩家系统
    - [x] 账户变动查询
    - [x] 分享玩家列表
- [x] 管理后台
    - [x] 调整玩家账户
    - [x] 查看充值明细
    - [x] 查看领红包明细
    - [x] 添加游戏城
## 设计思路
- 领域概念
 - 游戏城(GameCity) 
 - 房间(Room) 座位(Seat) 
 - 玩家(palyer) 
 - 游戏项目(GameProject) 
 - 一局游戏(InningeGame)
 - 城主(CityManager)    
 - 房主(RoomManager) 
 - 牌类生成器(cardsGame) 
 - 管理员(Manager)
> 管理员可以添加多个游戏城,每个游戏城有一个城主,城主可以配置自己管理的游戏城(是否开放,开房价格,加入密码,发布公告,对城内个房间推送消息)
> 玩家可以自己建立房间,每个房间有一个房主,房主可以配置房间(设置门票价格,房间公告,房间密码,设置房间人数,提出玩家,切换房间游戏项目等)
> 每个房间可以选择玩不同的游戏,房间有若干座位,只有进入房间并坐下的玩家能参与游戏.游戏以通常以一局为单位,由房主控制游戏开始.每局游戏结束后,玩家不用退出房间就可切换玩其他的游戏项目.

- 架构设计
  - 微信接口的集成
    利用微信用户的OPendId的唯一性实现登陆用户的身份的自动认证(通过微信手机客户端登陆不用输入密码),认证,支付,分享等调用微信接口.
  - Bs结构下持久化游戏状态
    游戏城,房间,座位,游戏项目,每局游戏均以静态对象方式存在,对于玩家只保留其Id和name,对玩家账户余额的查询和变更都实时连接数据库进行查询.
  - 服务器和客户端,客户端和客户端的通讯机制
    玩家客户端(微信内置浏览器)和服务器的通信混合采用ajax和webscocked方式.玩家进入房间就和服务器建立webscocket连接.服务器端使用websocket连接,单向向客户端推送数据,客户端不使用websocket向服务端发送数据,只使用ajax方式向服务器端发送数据和调用.
 >服务器端可以向指定客户端推送信息,调用客户的js方法并传递参数.客户端也可以以服务器端为中介间接向其他客户端发送数据和发起对其他客户端js的调用.