# 关键数据流

## [流程名称，例：用户下单]

### 触发条件
[例：用户在前端点击"提交订单"按钮]

### 调用链
```
[前端] POST /api/v1/orders
  → [api/routes] orderRouter.create
    → [modules/order] orderService.createOrder(userId, cartItems)
      → [modules/inventory] inventoryService.reserve(items)  // 副作用：锁定库存
      → [modules/payment] paymentService.initiate(order)     // 副作用：创建支付记录
    ← 返回 { orderId, paymentUrl }
  ← 响应 201 { code: 0, data: { orderId, paymentUrl } }
```

### 关键状态变更
| 步骤 | 数据表/存储 | 变更 |
|------|-------------|------|
| 1 | orders | 新增记录，状态=pending |
| 2 | inventory | 对应商品 reserved_qty += N |
| 3 | payments | 新增记录，状态=initiated |

### 失败处理
| 失败点 | 处理方式 |
|--------|----------|
| 库存不足 | 返回 400，不创建订单 |
| 支付创建失败 | 回滚库存预留，订单标记为 failed |

---

## [下一个流程...]

> 提示：每个关键业务流程记录一条数据流。重点关注跨模块调用、副作用和失败处理。
