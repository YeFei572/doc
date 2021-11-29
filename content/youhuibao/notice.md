需要讨论的相关点：

## 一、2021-11-14日
- [ ] 是否需要添加消息模块，包含用户推送消息（活动、订阅等）。商户推送消息（用户消费记录等）
- [ ] 关于权限设置，是否需要设置一店多管理的模式
- [ ] 关于图床问题，目前设置图床是SM.MS图床的可行性

## 二、2021-11-29日

> 关于数据库表的修改意见

- [ ] 处于对消费者和商户端的直观性考虑，建议消费流水表和充值流水表合二为一。因为到时候流入流出会在一个页面，切还要支持搜索功能，如果分开两张表，上述需求可能就不好做了。另外关于积分支付、线下支付等字段移除，这些都是订单里面需要注意的，包括优惠券信息，都是订单里面的，流水表只关心账户流入流出，至于流入流出的用途都关联在订单信息里面。如下图所示：
![](https://img.v2ss.cn/zatu/2021/11/29/09_29_15_f339ddcefe47100a1a6915c6b25a360.jpg)

```sql
DROP TABLE IF EXISTS `sys_laundry_record`;
CREATE TABLE `sys_laundry_record`  (
  `id` bigint(20) NOT NULL COMMENT '主键id',
  `user_id` bigint(20) NOT NULL COMMENT '流水所属用户id',
  `order_id` bigint(20) NULL DEFAULT NULL COMMENT '订单id，如果是充值，则该字段为空',
  `laundry_coin` decimal(10, 2) NOT NULL COMMENT '流水代币',
  `laundry_money` decimal(10, 2) NULL DEFAULT NULL COMMENT '流水金额，充值的时候不为空，消费为空',
  `laundry_type` tinyint(1) NOT NULL COMMENT '流水类型，1为消费，2为充值代币',
  `laundry_no` varchar(32) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '流水编号',
  `tenant_id` bigint(20) NOT NULL COMMENT '商户id',
   `remark` varchar(200) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '备注信息',
  `create_time` datetime NULL DEFAULT NULL COMMENT '创建时间',
  `update_time` datetime NULL DEFAULT NULL COMMENT '更新时间',
  `create_by` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '创建人',
  `update_by` varchar(30) CHARACTER SET utf8 COLLATE utf8_general_ci NULL DEFAULT NULL COMMENT '更新人',
  PRIMARY KEY (`id`) USING BTREE
) ENGINE = InnoDB CHARACTER SET = utf8 COLLATE = utf8_general_ci COMMENT = '流水表，本表含充值流水和消费流水，当为充值记录的时候则流水金额就是用户实缴人民币金额，当为消费的时候，则流水金额为空' ROW_FORMAT = Dynamic;

SET FOREIGN_KEY_CHECKS = 1;
```
