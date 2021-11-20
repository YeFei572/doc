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

