<!--
 * @Author: curechen 981470148@qq.com
 * @Date: 2023-01-30 16:43:58
 * @LastEditors: curechen 981470148@qq.com
 * @LastEditTime: 2023-03-16 17:52:24
 * @FilePath: \GraduationProject\uml\uml.md
 * @Description: 
-->
<!-- 用户端 -->
<!-- 添加购物车 -->
@startuml
actor       customer as user
participant CreateCart as c1
participant MallCartService as c2
participant buildMallCart as c3
participant Verify as c4
database    DB
user -> c1
c1-> c2: MallCartAddParam
c2->c3:req
c3->c4:CartAddParamVerify
c4-> DB: create
@enduml
<!-- 修改购物车 -->
@startuml
actor       customer as user
participant UpdateCart as c1
participant UpdateCartService as c2
participant buildMallCart as c3
participant Verify as c4
database    DB
user -> c1
c1-> c2: MallCartUpdateParam
c2->c3:req
c3->c4:CartUpdateParamVerify
c4-> DB: update
DB->c2: true
@enduml
<!-- 获取购物车 -->
@startuml
actor       customer as user
participant GetCartList as c1
participant GetCartInfoList as c2
participant buildMallCart as c3
database    DB
user -> c1
c1-> c2: pageInfo
c2->c3
c3-> DB: pageInfo
@enduml
<!-- 查看轮播图 -->
@startuml
actor       customer as user
participant GetCarouseList as c1
participant GetCarouseInfoList as c2
participant buildMallCarousel as c3
database    DB
user -> c1
c1-> c2: pageInfo
c2->c3
c3-> DB: pageInfo
@enduml
<!-- 查看推荐商品 -->
@startuml
actor       customer as user
participant GetRecommendList as c1
participant GetRecommendInfoList as c2
participant buildMallRecommend as c3
database    DB
user -> c1
c1-> c2: pageInfo
c2->c3
c3-> DB: pageInfo
DB->user: recommendList
@enduml
<!-- 修改用户昵称 -->
@startuml
actor       customer as user
participant UpdateCustomerUserName as c1
participant GetHeader as c2
participant updateMallCustomerName as c3
participant isLogin as c4
database    DB
user -> c1:MallUpdateNameParam
c1-> c2: token
c2->c3:token.req
c3->c4:checkToken
c4->user:false, unlogin
c4-> DB: true, update
@enduml
<!-- 修改登录密码 -->
@startuml
actor       customer as user
participant UpdateCustomerUserPassword as c1
participant GetHeader as c2
participant updateMallCustomerPassword as c3
participant isLogin as c4
database    DB
user -> c1:MallUpdatePasswordParam
c1-> c2: token
c2->c3:token.req
c3->c4:checkToken
c4->user:false, unlogin
c4-> DB: true, update
@enduml
<!-- 通过参数搜索商品 -->
@startuml
actor       customer as user
participant GetGoodsInfoList as c1
participant GetMallGoodsInfoList as c2
database    DB
user -> c1
c1-> c2: searchParam
c2->DB:req
DB->user:mallGoodsInfo
@enduml
<!-- 商品结算 -->
@startuml
actor       customer as user
participant SettlementProduct as c1
participant SettlementMallProduct as c2
database    DB
user -> c1
c1-> c2: orderParam
c2->DB:req
DB->user:success
@enduml
<!-- 用户注册 -->
@startuml
actor       admin       as user
participant CreateCustomerUser
participant Verify
participant CreateMallCustomerUser
database    DB
user -> CreateCustomerUser : MallCustomerUserParam 
CreateCustomerUser -> Verify : params, rule
Verify -> user : FailWithMessage
Verify -> CreateMallCustomerUser : MallCustomerUser
CreateMallCustomerUser -> DB : UserInfo
@enduml

<!-- 用户登录 -->
@startuml
actor       user       as user
participant UserLogin
database    DB
participant checkExists
participant setExpire
user -> UserLogin : MallCustomerUserLoginParam 
UserLogin -> DB : params 
DB -> UserLogin : exists
UserLogin -> checkExists : true
checkExists -> setExpire : expireTime
setExpire -> DB : update
@enduml

<!-- 管理端 -->
<!-- 查询用户订单 -->
@startuml
actor       admin as user
participant GetOrderInfoList
participant GetMallOrderInfoList
database    DB
user -> GetOrderInfoList: MallOrderParam 
GetOrderInfoList-> GetMallOrderInfoList: pageInfo,orderName
GetMallOrderInfoList-> DB 
@enduml

<!-- 修改订单状态 -->
@startuml
actor       admin as user
participant UpdateOrderInfo as c1
participant UpdateMallOrderInfo as c2
participant buildMallOrderInfo as c3
participant Verify as c4
database    DB
user -> c1 
c1-> c2: OrderInfoUpdateParam
c2-> c3: req
c3->c4: orderInfo
c4->DB: orderInfo, update
@enduml

<!-- 查看商品详情 -->
@startuml
actor       admin as user
participant GetGoodsInfo as c1
participant GetMallGoodsInfo as c2
database    DB
user -> c1 
c1-> c2: pageInfo,goodsName
c2-> DB: goods_name
DB-> user: mallGoodsInfo
@enduml

<!-- 封禁/解封商城用户 -->
@startuml
actor       admin as user
participant UnlockUser as c1
participant UnlockMallUser as c2
database    DB
user -> c1: userID
c1-> c2: userID
c2-> DB: req, params
@enduml