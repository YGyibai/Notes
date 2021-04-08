### 用户

用户表

名字	身份证 	密码 	真实姓名 	手机号	邮箱	注册时间	账号状态



购物车（订单）

序列号	商品id（多个）	用户id		

### 商家	

商家注册——》管理员审核 和登录

**商家表  shop**

| 字段名        | 数据类型    | 备注     |
| :------------ | :---------- | :------- |
| id            | int(11)     | 商家id   |
| name          | varchar(50) | 商家姓名 |
| real_name     | var         | 真实姓名 |
| ID_card       | varchar(50) | 身份证   |
| phone         | varchar(50) | 手机号   |
| email         | varchar(50) | 邮箱     |
| register_time | v           | 注册时间 |
| state         | int         | 账号状态 |
| shop_name    | varchar(50) | 店铺名称 |
| Shop_state   | int         | 店铺状态 0关闭 |

**商品表  goods**

名称	类别	详情 	图片地址（最多放入三张）描述   属性（商品属性）优先级别	店铺id	库存

| 字段名          | 数据类型    | 备注                                           |
| :-------------- | :---------- | :--------------------------------------------- |
| id              | int(11)     | 商品id                                         |
| shop_id         | varchar(50) | 店铺名称                                       |
| goods_name      | varchar(50) | 商品名称                                       |
| stock           | int         | 库存数量                                       |
| Sales           | Int         | 销售数量                                       |
| price           | double      | 销售价格                                       |
| real_price      | double      | 进价                                           |
| url             | v           | 图片地址                                       |
| state           | int         | 商品状态（上架/下架/审核中）0下架1上架2 审核中 |
| type_id         | int         | 商品类别id                                     |
| recommend_goods | tinyint     | 是否是推荐 1true 0flase                        |

**商品属性 goods_attribute**

商品id	公共属性（男/女）销售属性（春夏秋冬）关键属性（品牌）

| 字段名       | 数据类型     | 备注             |
| :----------- | :----------- | :--------------- |
| goods_id     | int(11)      | 商品id           |
| introduction | varchar(255) | 商品简介，促销语 |
| Common_attr  | varchar(50)  | 公共属性         |
| sale_attr    | varchar(255) | 销售属性         |
| crux_attr    | Var          | 关键属性         |
| url          |              |                  |
| url          |              |                  |
| url          |              |                  |

**商品类别 goods_type**

| 字段名     | 数据类型    | 备注                       |
| :--------- | :---------- | :------------------------- |
| id         | int(11)     | 类别id                     |
| type_name  | varchar(50) | 类别名称                   |
| is_visible | tinyint     | 是否显示1显示 0不显示默认1 |

**订单表 order_table**

| 字段名       | 数据类型    | 备注            |
| :----------- | :---------- | :-------------- |
| id           | int(11)     |                 |
| order_no     | varchar(50) | 订单编号        |
| user_id      | int         | 用户id          |
| user_message | v           | 用户评论        |
| user_phone   | v           | 用户手机号      |
| address      | v           | 收货地址        |
| shop_id      |             | 商店id          |
| shop_name    |             | 店铺名字        |
| Goods_id     |             |                 |
| goods_name   |             | 商品名          |
| goods_number |             | 数量            |
| order_price  |             | 金额            |
| create_time  | date        | 订单创建时间    |
| finish_time  | date        | 订单完成时间    |
| order_state  | int         | 0未完成1 已完成 |
| refund_money | double      | 退款金额        |

​	**收货单receipt**

| 字段名       | 数据类型    | 备注     |
| :----------- | :---------- | :------- |
| id           | int(11)     |          |
| User_id      | varchar(50) | 用户编号 |
| user_name    | va          | 用户姓名 |
| user_phone   | v           | 手机号   |
| User_address | v           | 用户地址 |

**购物车 shopCar**

| 字段名  | 数据类型 | 备注              |
| :------ | :------- | :---------------- |
| id      | int(11)  |                   |
| carMap  | Varchar  | {商品id:商品数量} |
| user_id | int(11)  | 用户id            |

