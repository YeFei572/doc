> 本页面主要记载接口需求方面的说明，按照模块和日期为`Title`

### 一、商品分类

> 商品分类模块，和商品一对一关系。

#### 1、表结构设计：

```sql
-- ----------------------------
-- Table structure for prod_category
-- ----------------------------
DROP TABLE IF EXISTS `prod_category`;
CREATE TABLE `prod_category`  (
  `id` bigint(20) NOT NULL COMMENT '主键id',
  `category_name` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NOT NULL COMMENT '分类名称',
  `icon_id` bigint(20) NULL DEFAULT NULL COMMENT '分类图标，（备用）',
  `create_time` datetime(0) NULL DEFAULT NULL COMMENT '创建时间',
  `create_by` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '创建人',
  `update_time` datetime(0) NULL DEFAULT NULL COMMENT '更新时间',
  `update_by` varchar(100) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '更新人',
  `del_flag` tinyint(1) NOT NULL DEFAULT 0 COMMENT '是否删除，0未删除，1已删除，默认0',
  `tenant_id` bigint(20) NOT NULL COMMENT '商户id',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '商品分类表，icon字段做备用，暂时可以先不管' ROW_FORMAT = Dynamic;

SET FOREIGN_KEY_CHECKS = 1;
```
以上设计其中的`icon_id`是表格`prod_picture`的主键id，分类的图标，暂时先不用管，为了以后的扩展预留字段

#### 2、接口需求
- [ ] 商品分类增加
- [ ] 商品分类删除
- [ ] 商品分类编辑
- [ ] 商品分类查询 

### 二、商品管理
> 商品管理包含了商品`prod_item`和商品规格`prod_category`两个模块，商品和商品规格两个接口是分开的，包含增删改查。

#### 1、表结构设计
```sql
DROP TABLE IF EXISTS `pro_item`;
CREATE TABLE `pro_item`  (
  `id` bigint(20) NOT NULL COMMENT '主键id',
  `category_id` bigint(20) NOT NULL COMMENT '分类id',
  `prod_name` varchar(100) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NOT NULL COMMENT '产品名字',
  `price` decimal(10, 2) NULL DEFAULT NULL COMMENT '价钱',
  `prod_desc` longtext CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL COMMENT '描述',
  `sku` int(10) NULL DEFAULT NULL COMMENT '库存(限量库存)',
  `publish_flag` tinyint(1) NULL DEFAULT NULL COMMENT '是否发布',
  `publish_time` datetime(0) NULL DEFAULT NULL COMMENT '发布时间',
  `create_by` varchar(100) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '创建者',
  `create_time` datetime(0) NULL DEFAULT NULL COMMENT '创建时间',
  `update_by` varchar(100) CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci NULL DEFAULT NULL COMMENT '更新者',
  `update_time` datetime(0) NULL DEFAULT NULL COMMENT '更新时间',
  `del_flag` tinyint(1) NOT NULL DEFAULT 0 COMMENT '是否删除，0未删除，1已删除，默认0',
  `tenant_id` bigint(20) NOT NULL COMMENT '商户id',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8mb4 COLLATE = utf8mb4_general_ci COMMENT = '商品详情' ROW_FORMAT = Dynamic;

SET FOREIGN_KEY_CHECKS = 1;
```

#### 1、商品模块
- [ ] 商品接口增加
```json
{
    "id": 123,                                    // 商品id
	"prod_name": "青椒炒肉丝",                     // 商品名称
	"prod_desc": "新鲜青椒炒肉丝，好吃顺滑",        // 商品描述
	"pictures": [                                 // 商品banner图片列表
		"https://files.catbox.moe/gobjdy.png",
		"https://files.catbox.moe/7k9nx5.png",
		"https://files.catbox.moe/abu8sb.png"
	],
	"price": 20.5,                              // 商品价格
	"category_id": 111111111111111,             // 商品分类id
	"category_name": "美食",                     // 商品分类名称
	"sku": 100                                // 商品总库存量，-1为不限量
}
```
- [ ] 商品接口删除
- [ ] 商品接口编辑
```json
{
    "id": 123,                                    // 商品id
	"prod_name": "青椒炒鸡蛋",                     // 商品名称
	"prod_desc": "新鲜青椒炒鸡蛋，好吃顺滑",        // 商品描述
	"pictures": [                                 // 商品banner图片列表
		"https://files.catbox.moe/gobjdy.png",
		"https://files.catbox.moe/7k9nx5.png",
		"https://files.catbox.moe/abu8sb.png"
	],
	"price": 30.5,                              // 商品价格
	"category_id": 222222222222222,             // 商品分类id
	"category_name": "美食",                     // 商品分类名称
	"sku": 200                                  // 商品总库存量，-1为不限量
}
```
- [ ] 商品接口查询
```json
{
    "code": 200,
    "msg": "查询成功",
    "data":[
        {
            "id": 123,                                    // 商品id
            "prod_name": "青椒炒鸡蛋",                     // 商品名称
            "prod_desc": "新鲜青椒炒鸡蛋，好吃顺滑",        // 商品描述
            "pictures": [                                 // 商品banner图片列表
                "https://files.catbox.moe/gobjdy.png",
                "https://files.catbox.moe/7k9nx5.png",
                "https://files.catbox.moe/abu8sb.png"
            ],
            "price": 30.5,                              // 商品价格
            "category_id": 222222222222222,             // 商品分类id
            "category_name": "美食",                     // 商品分类名称
            "sku": 200                                  // 商品总库存量，-1为不限量
        }
    ]
}
```