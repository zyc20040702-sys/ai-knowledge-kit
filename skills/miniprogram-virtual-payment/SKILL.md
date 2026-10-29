---
name: 微信小程序虚拟支付
description: 微信小程序虚拟支付接入（含个人主体）。当需要接入虚拟支付、道具直购、代币充值、处理 paySig/signature、发货回调或排查支付报错时使用。
argument-hint: [问题描述或当前阶段]
allowed-tools: Read Grep WebFetch
---

# 微信小程序虚拟支付 · 开发者接入 Skill

> 这是微信小程序虚拟支付的完整接入指引，帮助开发者从零到一完成虚拟支付的接入。

---

## 技能目标

为开发者提供一套可直接执行的微信小程序虚拟支付接入流程，帮助快速完成从配置到上线的闭环。

**覆盖能力**：
- 道具直购模式（直接购买虚拟商品）
- 代币充值模式（充值虚拟货币后消费）
- iOS 端接入（自动路由至 Apple IAP）
- 签名计算、回调处理、幂等发货

**适用场景**：
- 设计或实现虚拟支付接入方案
- 排查支付失败、签名失败、回调异常
- 制定沙箱联调和正式上线清单

---


## 个人主体补充（本工作区适用）

当前用户小程序主体为**个人**。开通以官方「虚拟支付：个人」为准（通常需认证、备案，类目含工具）。下文企业资质条目对个人不强制；冲突时以个人文档为准：https://developers.weixin.qq.com/miniprogram/dev/platform-capabilities/business-capabilities/virtual-payment/person

## 必要输入

执行前优先收集以下信息：

| 信息项 | 说明 | 示例 |
|--------|------|------|
| 业务模式 | 道具直购 / 代币充值 | `short_series_goods` |
| 当前阶段 | 新接入 / 联调排错 / 上线准备 | 联调中 |
| 服务端语言 | Node.js / Python / Java / Go | Node.js |
| 是否已开通虚拟支付 | 是 / 否 | 是 |
| 是否需要 iOS 支付 | 是 / 否 | 是 |

若关键信息缺失，只补问最少问题。

---

## 权威文档来源

默认以微信官方文档为准：

- **概览**：https://developers.weixin.qq.com/miniprogram/dev/platform-capabilities/business-capabilities/virtual-payment.html
- **开发接入**：https://developers.weixin.qq.com/miniprogram/dev/platform-capabilities/business-capabilities/virtual-payment/devplan.html
- **iOS 接入**：https://developers.weixin.qq.com/miniprogram/dev/platform-capabilities/business-capabilities/virtual-payment/ios.html
- **API 文档**：https://developers.weixin.qq.com/miniprogram/dev/api/payment/wx.requestVirtualPayment.html

---

## 标准执行流程

请在回复中复制并维护以下清单：

```markdown
## 接入进度
- [ ] 1. 确认业务模式和接入范围
- [ ] 2. 校验接入前置条件
- [ ] 3. 完成后台配置（道具/代币/回调地址）
- [ ] 4. 完成服务端签名接口
- [ ] 5. 完成前端支付调用
- [ ] 6. 完成回调处理与幂等发货
- [ ] 7. 完成错误码映射和降级策略
- [ ] 8. 完成沙箱联调验证
- [ ] 9. 完成 iOS 端适配（如需）
- [ ] 10. 完成正式上线检查
```

---

## 1. 接入前自检清单

在开始之前，请逐项确认：

| # | 检查项 | 状态 |
|---|--------|------|
| 1 | 小程序已完成**企业认证**（企业 / 事业单位 / 个体工商户） | ☐ |
| 2 | 小程序主体信息完备 | ☐ |
| 3 | 你卖的是**虚拟商品**（如解锁内容、代币等，不是实物） | ☐ |
| 4 | 已准备好：企业营业执照、提现银行账户信息、支付管理员身份信息 | ☐ |

> 💡 **实物商品**用的是普通的微信支付，不需要接虚拟支付。虚拟支付专门用于虚拟商品。

---

## 2. 开通虚拟支付

### 操作路径

登录 [微信公众平台](https://mp.weixin.qq.com) → 左侧菜单【虚拟支付】→ 开始开通

### 流程

```
阅读协议 → 提交资料 → 等待审核（1-7 个工作日）→ 签约 → 完成
```

### 详细步骤

1. **阅读并同意协议**
2. **提交资料**：
   - 企业营业执照
   - 提现账户信息（银行卡）
   - 支付管理员信息
3. **等待审核**：1~7 个工作日
4. **签约**：
   - 如果支付管理员**是法人** → 直接扫码签约
   - 如果支付管理员**不是法人** → 先做账户验证，再签约
5. **等待 1~2 个工作日**，状态显示「已签约」即开通成功 ✅

### 开通后你会获得

| 参数 | 说明 | 在哪里找 |
|------|------|---------|
| `appid` | 你的小程序 ID | 小程序后台 → 开发 → 开发管理 |
| `offerId` | 虚拟支付应用 ID | 虚拟支付 → 基础配置 |
| `appKey` | 支付签名密钥 | 虚拟支付 → 基础配置 |

> ⚠️ **`appKey` 是签名的核心密钥，必须保存在服务端，绝对不能放在前端代码中！**

---

## 3. 后台配置

### 配置代币（如果你用代币模式）

进入：虚拟支付 → 基础配置 → 代币配置

| 配置项 | 说明 | 举例 |
|--------|------|------|
| 代币名称 | 你的虚拟货币叫什么 | 金币、钻石、书币 |
| 兑换比例 | 1 元人民币 = 多少代币 | 1元 = 100 金币 |

> ⚠️ **代币名称和兑换比例一旦发布就无法修改！** 请想清楚再发布。

### 配置道具（如果你用道具直购模式）

进入：虚拟支付 → 基础配置 → 道具配置

| 配置项 | 说明 | 举例 |
|--------|------|------|
| 道具 ID（`productId`） | 唯一标识 | `vip_monthly`、`chapter_unlock` |
| 道具名称 | 展示给用户看的名字 | VIP 权益、章节解锁 |
| 道具价格 | 单位是**分** | 600（= 6 元） |

### 配置回调地址

进入：虚拟支付 → 基础配置 → 消息推送配置

把你服务端接收回调通知的 URL 填进去（如 `https://api.yourapp.com/wx/callback`）。

---

## 4. 选择业务模式

微信虚拟支付有两种模式，根据你的业务选择：

### 模式 A：道具直购（推荐新手）

```
用户选择商品 → 直接付钱 → 你发货
```

- **适合**：解锁章节、购买皮肤、一次性功能等
- **mode 值**：`short_series_goods`
- **特点**：简单直观，用户付钱你就发货

### 模式 B：代币充值

```
用户先充值代币 → 代币到账 → 用代币买东西
```

- **适合**：需要虚拟货币体系的场景（如小说/短剧按章节付费）
- **mode 值**：`short_series_coin`
- **特点**：微信自动管理代币余额，你只需处理代币消费

### 怎么选？

| 场景 | 推荐模式 |
|------|---------|
| 解锁单个功能 / 章节 | 道具直购 |
| 一次性购买 VIP 权益 | 道具直购 |
| 小说按章收费 | 代币充值 |
| 短剧按集收费 | 代币充值 |
| 游戏内购 | 代币充值 |

---

## 5. 前端开发

### 5.1 版本兼容检查

**最低要求**：基础库 ≥ `2.19.2`

在调用支付 API 前，先检查用户的微信版本是否支持：

```javascript
// utils/version.js —— 版本号比较工具函数
function compareVersion(v1, v2) {
  if (typeof v1 !== 'string' || typeof v2 !== 'string') return 0
  const arr1 = v1.split('.')
  const arr2 = v2.split('.')
  const len = Math.max(arr1.length, arr2.length)
  while (arr1.length < len) arr1.push('0')
  while (arr2.length < len) arr2.push('0')
  for (let i = 0; i < len; i++) {
    const num1 = parseInt(arr1[i], 10)
    const num2 = parseInt(arr2[i], 10)
    if (num1 > num2) return 1
    if (num1 < num2) return -1
  }
  return 0
}

// 检查是否支持虚拟支付
function canUseVirtualPayment() {
  const { SDKVersion } = wx.getSystemInfoSync()
  return compareVersion(SDKVersion, '2.19.2') >= 0
    || wx.canIUse('requestVirtualPayment')
}

module.exports = { compareVersion, canUseVirtualPayment }
```

### 5.2 发起支付（核心代码）

```javascript
// pages/pay/pay.js

const { canUseVirtualPayment } = require('../../utils/version')

Page({
  // 点击购买按钮
  async handleBuy() {
    // 1. 检查版本
    if (!canUseVirtualPayment()) {
      wx.showToast({ title: '请升级微信后再试', icon: 'none' })
      return
    }

    // 2. 向你的服务端请求订单参数和签名
    const orderInfo = await this.createOrder()

    // 3. 调用虚拟支付 API
    wx.requestVirtualPayment({
      // ⚠️ signData 必须是 JSON.stringify 后的字符串
      signData: JSON.stringify({
        offerId: orderInfo.offerId,       // 从后台获取
        buyQuantity: 1,                    // 购买数量
        env: 0,                            // 0=正式环境, 1=沙箱测试
        currencyType: 'CNY',               // 固定填 CNY
        productId: orderInfo.productId,    // 道具 ID（道具直购必填）
        goodsPrice: orderInfo.goodsPrice,  // 道具价格，单位：分
        outTradeNo: orderInfo.outTradeNo,  // 你的业务订单号
        attach: orderInfo.attach || '',    // 透传数据，回调时原样返回给你
      }),
      paySig: orderInfo.paySig,            // 服务端计算的支付签名
      signature: orderInfo.signature,      // 服务端计算的用户态签名
      mode: 'short_series_goods',          // 道具直购
      // mode: 'short_series_coin',        // 如果是代币充值，改成这个

      success(res) {
        console.log('支付成功！', res)
        wx.showToast({ title: '购买成功', icon: 'success' })
        // ⚠️ 不要在这里发货！在服务端收到回调后再发货
      },
      fail({ errMsg, errCode }) {
        console.error('支付失败', errCode, errMsg)
        if (errCode === -2) {
          // 用户取消支付，不用报错
          return
        }
        wx.showToast({ title: '支付失败，请重试', icon: 'none' })
      }
    })
  },

  // 向你的服务端创建订单
  async createOrder() {
    return new Promise((resolve, reject) => {
      wx.request({
        url: 'https://api.yourapp.com/order/create',
        method: 'POST',
        data: {
          productId: 'your_product_id',
          // ... 其他业务参数
        },
        success(res) { resolve(res.data) },
        fail(err) { reject(err) }
      })
    })
  }
})
```

### 5.3 API 参数速查表

#### signData 对象结构

| 属性 | 类型 | 必填 | 说明 |
|------|------|------|------|
| `offerId` | string | ✅ 是 | 虚拟支付应用 ID，从后台"基础配置"获取 |
| `buyQuantity` | number | ✅ 是 | 购买数量 |
| `env` | number | 否 | 环境：`0`=正式（默认），`1`=沙箱 |
| `currencyType` | string | ✅ 是 | 币种，固定填 `CNY` |
| `productId` | string | 道具直购必填 | 道具 ID |
| `goodsPrice` | number | 道具直购必填 | 道具单价（单位：分） |
| `activitySellingPrice` | number | 否 | 道具优惠价格（分），最低为原价 40% |
| `outTradeNo` | string | ✅ 是 | 业务订单号，8-32字符，每单只能用一次 |
| `attach` | string | ✅ 是 | 透传数据，发货通知时原样返回 |

#### mode 合法值

| 值 | 说明 |
|----|------|
| `short_series_goods` | 道具直购 |
| `short_series_coin` | 代币充值 |

### 5.4 关于 outTradeNo（订单号）

| 规则 | 说明 |
|------|------|
| 长度 | 8~32 个字符 |
| 字符 | 数字、大小写字母、`_-\|*@` |
| 唯一性 | 每个订单号只能用一次，重复使用会报 `-15002` |
| 前缀限制 | **不能以下划线 `_` 开头** |

推荐格式：`ORD-{时间戳}-{随机数}`，如 `ORD-1711785600-a3b2c1`

---

## 6. 服务端开发

### 6.1 签名计算（最关键！）

虚拟支付有两种签名，都用 **HMAC-SHA256** 算法：

#### 支付签名（paySig）

```
paySig = HMAC-SHA256(appKey, uri + '&' + signData)
```

- `appKey`：从虚拟支付后台获取的密钥
- `uri`：API 路径（如 `/wxa/game/getbalance`）
- `signData`：前端传给 `wx.requestVirtualPayment` 的 signData（JSON 字符串）

#### 用户态签名（signature）

```
signature = HMAC-SHA256(sessionKey, signData)
```

- `sessionKey`：用户登录时通过 `wx.login` → `code2Session` 获取的
- `signData`：同上

#### Node.js 签名代码

```javascript
const crypto = require('crypto')

// 生成支付签名
function generatePaySig(appKey, uri, signData) {
  const message = uri + '&' + signData
  return crypto
    .createHmac('sha256', appKey)
    .update(message)
    .digest('hex')
}

// 生成用户态签名
function generateSignature(sessionKey, signData) {
  return crypto
    .createHmac('sha256', sessionKey)
    .update(signData)
    .digest('hex')
}
```

#### Python 签名代码

```python
import hmac
import hashlib

def generate_pay_sig(app_key: str, uri: str, sign_data: str) -> str:
    """生成支付签名"""
    message = uri + '&' + sign_data
    return hmac.new(
        app_key.encode('utf-8'),
        message.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()

def generate_signature(session_key: str, sign_data: str) -> str:
    """生成用户态签名"""
    return hmac.new(
        session_key.encode('utf-8'),
        sign_data.encode('utf-8'),
        hashlib.sha256
    ).hexdigest()
```

#### Java 签名代码

```java
import javax.crypto.Mac;
import javax.crypto.spec.SecretKeySpec;
import java.nio.charset.StandardCharsets;

public class VirtualPaymentSigner {
    
    // 生成支付签名
    public static String generatePaySig(String appKey, String uri, String signData) {
        String message = uri + "&" + signData;
        return hmacSha256(appKey, message);
    }
    
    // 生成用户态签名
    public static String generateSignature(String sessionKey, String signData) {
        return hmacSha256(sessionKey, signData);
    }
    
    private static String hmacSha256(String key, String data) {
        try {
            Mac mac = Mac.getInstance("HmacSHA256");
            SecretKeySpec secretKeySpec = new SecretKeySpec(
                key.getBytes(StandardCharsets.UTF_8), "HmacSHA256");
            mac.init(secretKeySpec);
            byte[] hash = mac.doFinal(data.getBytes(StandardCharsets.UTF_8));
            StringBuilder hexString = new StringBuilder();
            for (byte b : hash) {
                String hex = Integer.toHexString(0xff & b);
                if (hex.length() == 1) hexString.append('0');
                hexString.append(hex);
            }
            return hexString.toString();
        } catch (Exception e) {
            throw new RuntimeException("签名计算失败", e);
        }
    }
}
```

#### Go 签名代码

```go
package main

import (
    "crypto/hmac"
    "crypto/sha256"
    "encoding/hex"
)

// GeneratePaySig 生成支付签名
func GeneratePaySig(appKey, uri, signData string) string {
    message := uri + "&" + signData
    return hmacSha256(appKey, message)
}

// GenerateSignature 生成用户态签名
func GenerateSignature(sessionKey, signData string) string {
    return hmacSha256(sessionKey, signData)
}

func hmacSha256(key, data string) string {
    h := hmac.New(sha256.New, []byte(key))
    h.Write([]byte(data))
    return hex.EncodeToString(h.Sum(nil))
}
```

### 6.2 创建订单接口示例（Node.js / Express）

```javascript
const express = require('express')
const crypto = require('crypto')
const router = express.Router()

// 你的配置（从后台获取）
const CONFIG = {
  appId: 'wx1234567890',
  offerId: '你的offerId',
  appKey: '你的appKey',
}

// 创建订单
router.post('/order/create', async (req, res) => {
  const { productId, openid } = req.body

  // 1. 查询道具信息（从你的数据库）
  const product = await getProduct(productId)

  // 2. 生成唯一订单号
  const outTradeNo = `ORD${Date.now()}${Math.random().toString(36).slice(2, 8)}`

  // 3. 构造 signData
  const signDataObj = {
    offerId: CONFIG.offerId,
    buyQuantity: 1,
    env: 0,
    currencyType: 'CNY',
    productId: product.id,
    goodsPrice: product.price, // 单位：分
    outTradeNo,
    attach: JSON.stringify({ userId: req.userId }),
  }
  const signData = JSON.stringify(signDataObj)

  // 4. 计算签名
  const uri = '/wxa/game/getbalance' // 虚拟支付统一 uri
  const paySig = generatePaySig(CONFIG.appKey, uri, signData)

  // 5. 获取用户 sessionKey（你需要在登录时缓存）
  const sessionKey = await getUserSessionKey(openid)
  const signature = generateSignature(sessionKey, signData)

  // 6. 保存订单到你的数据库
  await saveOrder({ outTradeNo, productId, openid, price: product.price })

  // 7. 返回给前端
  res.json({
    offerId: CONFIG.offerId,
    productId: product.id,
    goodsPrice: product.price,
    outTradeNo,
    attach: signDataObj.attach,
    paySig,
    signature,
  })
})
```

### 6.3 服务端核心 API 速查

| 接口 | 用途 | 什么时候用 |
|------|------|-----------|
| `query_order` | 查询订单状态 | 主动确认用户是否已支付 |
| `notify_provide_goods` | 确认发货 | 收到发货通知后，你发完货调用这个 |
| `currency_pay` | 扣减代币 | 代币模式下，用户消费代币时 |
| `cancel_currency_pay` | 代币退款 | 代币扣错了，需要退回 |
| `refund_order` | 现金退款 | 道具直购的退款 |
| `query_user_balance` | 查用户代币余额 | 展示用户代币余额 |

---

## 7. 处理回调通知

### 7.1 为什么必须处理回调？

> ⚠️ **前端 `success` 回调不可靠！** 如果用户支付成功后微信崩溃或断网，前端不会收到成功回调。
> **必须**在服务端处理微信推送的回调通知来确认发货。

### 7.2 你需要处理的核心回调

| 回调事件 | 触发时机 | 你要做什么 |
|---------|---------|-----------|
| `xpay_goods_deliver_notify` | 用户支付成功 | **发货**（给用户加道具/权限），然后调 `notify_provide_goods` 确认 |
| `xpay_coin_pay_notify` | 代币扣减成功 | 记录消费，给用户发放对应道具 |
| `xpay_refund_notify` | 退款完成 | 回收道具/权限 |
| `xpay_complaint_notify` | 用户投诉 | 24 小时内处理 |

### 7.3 回调处理代码示例（Node.js）

```javascript
// 接收微信回调
router.post('/wx/callback', async (req, res) => {
  const { Event } = req.body

  switch (Event) {
    case 'xpay_goods_deliver_notify':
      await handleDeliverNotify(req.body)
      break
    case 'xpay_refund_notify':
      await handleRefundNotify(req.body)
      break
    case 'xpay_complaint_notify':
      await handleComplaintNotify(req.body)
      break
  }

  // ✅ 必须返回这个格式，否则微信会重复推送
  res.json({ ErrCode: 0, ErrMsg: 'success' })
})

// 处理发货通知
async function handleDeliverNotify(data) {
  const { outTradeNo, productId, buyQuantity, openid, attach } = data

  // 1. 幂等检查：这个订单是不是已经发过货了？
  const order = await getOrder(outTradeNo)
  if (order.status === 'delivered') {
    return // 已发货，直接返回，不重复发
  }

  // 2. 执行你的发货逻辑（给用户增加道具/权限）
  await deliverGoods(openid, productId, buyQuantity)

  // 3. 更新订单状态
  await updateOrderStatus(outTradeNo, 'delivered')

  // 4. 调用微信接口确认发货
  await notifyProvideGoods(outTradeNo)
}
```

### 7.4 额外保障：主动轮询

除了被动接收回调，建议你还实现一个**主动轮询**机制：

```javascript
// 支付完成后启动轮询（前端或服务端定时任务都可以）
async function pollOrderStatus(outTradeNo, maxRetries = 10) {
  for (let i = 0; i < maxRetries; i++) {
    const result = await queryOrder(outTradeNo)
    if (result.status === 'SUCCESS') {
      // 订单已支付，执行发货
      await deliverGoods(result)
      return
    }
    // 等待后重试（递增间隔）
    await sleep(Math.min(3000 * (i + 1), 30000))
  }
  // 超过重试次数，标记为异常
  console.error(`订单 ${outTradeNo} 轮询超时，需人工处理`)
}
```

---

## 8. 测试验证

### 8.1 使用沙箱环境测试

| 参数 | 测试时 | 上线时 |
|------|-------|--------|
| `env` | `1`（沙箱） | `0`（正式） |

沙箱环境**不会真扣钱**，放心测试。

> ⚠️ **iOS 不支持沙箱测试**，只能用正式环境（`env=0`）。

### 8.2 测试检查清单

| # | 测试项 | 通过 |
|---|--------|------|
| 1 | 能正常弹出支付界面 | ☐ |
| 2 | 支付成功后前端收到 success 回调 | ☐ |
| 3 | 服务端收到 `xpay_goods_deliver_notify` 回调 | ☐ |
| 4 | 用户支付后能正确收到道具/代币 | ☐ |
| 5 | 重复发送同一个 outTradeNo 会报错 `-15002` | ☐ |
| 6 | 签名错误时能看到 `-15005` 或 `-15006` | ☐ |
| 7 | 用户取消支付时前端 errCode 为 `-2` | ☐ |
| 8 | 模拟回调丢失场景，轮询机制能正常工作 | ☐ |

### 8.3 调试技巧

1. **签名问题排查**：用上面的签名代码单独算一遍，对比前端传的值
2. **回调收不到**：检查后台配置的回调 URL 是否正确、服务器是否公网可访问
3. **道具/代币报错**：确认已在后台发布，且等待至少 **10 分钟**生效

---

## 9. iOS 端接入

> iOS 用户的支付走的是 **Apple IAP（应用内购买）**，微信会自动路由，你的代码基本不用改。

### 前提条件

| 条件 | 说明 |
|------|------|
| 已开通虚拟支付 | 基础虚拟支付已签约 |
| 已配置小程序简称 | 微信公众平台 → 虚拟支付 → 基础配置 |

### iOS 和 Android 的关键差异

| 对比项 | Android | iOS |
|--------|---------|-----|
| 支付系统 | 普通虚拟支付 | Apple IAP |
| 沙箱测试 | ✅ 支持（`env=1`） | ❌ 不支持，只能用 `env=0` |
| 主动退款 | ✅ 你可以退 | ❌ 你退不了，用户要去 App Store 申请 |
| 结算周期 | T+3（3天到账） | 月结后 30~50 天 |
| 最低金额 | 无特殊限制 | **1 元** |
| 客户端要求 | 基础库 ≥ 2.19.2 | 微信 ≥ 8.0.68，iOS ≥ 15 |

### 代码上需要注意什么？

**几乎不需要改代码！** 同一个 `wx.requestVirtualPayment` API，微信会根据用户设备自动路由到对应的支付系统。

唯一注意点：**iOS 端 `env` 只能是 `0`**（不支持沙箱），所以如果你之前用 `env: 1` 测试，上线时记得改成 `0`。

### iOS 退款处理

iOS 退款比较特殊，流程是：

```
用户在 App Store 申请退款 → Apple 问你"建不建议退？" → 你3秒内回复 → Apple 最终决定
```

你需要处理 `xpay-ios-refund-query-notify` 回调：

```javascript
// iOS 退款问询处理
async function handleIosRefundQuery(data) {
  const { pay_order_id, provide_status, refund_request_reason } = data

  // 根据业务逻辑决定是否建议退款
  const response = {
    result_code: 0,     // 0=建议退款, 1=拒绝退款
    result_info: '同意退款',
    evidence: `订单${pay_order_id}，用户原因：${refund_request_reason}`,
  }

  return response
}
```

> ⚠️ **必须在 3 秒内响应！** Apple 会连续问询 3 次，如果都超时，退款决定权完全交由 Apple。

---

## 10. 错误码速查表

### 前端错误码（完整版）

| 错误码 | 错误信息 | 说明 | 解决方案 |
|--------|----------|------|----------|
| `-1` | 支付失败 | 通用支付失败 | 根据 errMsg 排查具体原因 |
| `-2` | 支付取消 | 用户主动取消 | 正常情况，不用处理 |
| `-4` | 风控拦截 | 触发风控规则 | 联系客服了解详情 |
| `-5` | 开通签约结果未知 | 签约状态不确定 | 重试或联系支持 |
| `1001` | 参数错误 | 通用参数错误 | 检查所有参数格式 |
| `-15001` | 参数错误 | 具体原因见 errMsg | 根据 errMsg 排查 |
| `-15002` | outTradeNo 重复使用 | 订单号已用过 | **换一个新的 outTradeNo** |
| `-15003` | 系统错误 | 微信系统异常 | 稍后重试 |
| `-15004` | currencyType 错误 | 币种错误 | 只能填 `CNY` |
| `-15005` | 用户态签名 signature 错误 | sessionKey 可能过期 | **重新 `wx.login`** 获取新 sessionKey |
| `-15006` | 支付签名 paySig 错误 | 签名计算有误 | **检查 appKey、uri、signData** |
| `-15007` | session_key 过期 | 登录态失效 | **重新 `wx.login`** |
| `-15008` | 二级商户进件未完成 | 服务商模式问题 | 完成商户进件 |
| `-15009` | 代币未发布 | 代币配置未发布 | **去后台发布代币** |
| `-15010` | 道具 productId 未发布 | 道具配置未发布 | **去后台发布道具** |
| `-15011` | 现网版本的 env 只能是 0 | iOS 不支持沙箱 | **iOS 端 env 改为 0** |
| `-15012` | 调用失败导致关单 | 内部错误 | **换新订单号重试** |
| `-15013` | goodsPrice 道具价格错误 | 价格不匹配 | **检查前端传的价格和后台配置** |
| `-15014` | 道具/代币发布未生效 | 刚发布还没生效 | **等 10 分钟后再试** |
| `-15016` | signData 格式有问题 | JSON 格式错误 | 检查 `JSON.stringify` 是否正确 |
| `-15017` | 此商家涉嫌违规 | 收款功能被限制 | 登录微信商户平台查看原因 |
| `-15018` | 代币/道具审核不通过 | 配置审核失败 | 修改配置后重新提交 |
| `-15019` | 调微信报商户受限 | 商户被限制 | 登录商户平台查看解决方案 |
| `-15020` | 操作过快 | 频率限制 | **稍后重试** |
| `-15021` | 小程序被限频交易 | 小程序被限制 | 联系微信支持 |

### 服务端常见错误

| 错误码 | 意思 | 怎么解决 |
|--------|------|---------|
| `268490001` | openid 错误 | 检查 openid |
| `268490003` | 签名错误 | 验证 appKey、uri、signData |
| `268490009` | session_key 过期 | 重新获取登录态 |
| `268490015` | 频率限制 | 降低调用频率 |

---

## 11. 费率与结算

关于虚拟支付的费率、分成比例、结算周期等信息，请查阅官方最新说明：

👉 **[虚拟支付费率与结算说明](https://developers.weixin.qq.com/miniprogram/dev/platform-capabilities/business-capabilities/virtual-payment/devplan.html)**

> **注意**：费率可能会随政策调整，请以官方文档为准。

---

## 12. 常见踩坑与 FAQ

### Q1：goodsPrice 单位是什么？
**分！不是元！** 600 代表 6 元，不是 600 元。

### Q2：signData 为什么要 JSON.stringify？
接口设计要求以字符串形式传递，这样签名计算和数据传输才能保持一致。

### Q3：支付成功了但用户没收到道具？
最可能的原因是你只依赖了前端 `success` 回调来发货。前端回调不可靠，**必须在服务端处理 `xpay_goods_deliver_notify` 回调来发货**。

### Q4：签名一直报错怎么排查？
1. 确认 `appKey` 来自虚拟支付后台（不是微信支付的密钥）
2. 确认 `uri` 不包含域名和查询参数
3. 确认 `signData` 和传给 `wx.requestVirtualPayment` 的完全一致
4. 确认 `sessionKey` 没过期（有效期可能很短，建议每次支付前重新 login）

### Q5：代币名称/兑换比例能改吗？
**不能！** 一旦发布就无法修改。务必在发布前想清楚。

### Q6：iOS 端能用沙箱测试吗？
**不能。** iOS 只支持正式环境（`env=0`）。测试时需要用真实的 Apple 支付（但可以小金额测试）。

### Q7：正式上线前的最终检查？
1. `env` 改成 `0`
2. 回调 URL 指向正式服务器
3. 密钥没有暴露在前端
4. 幂等处理和轮询机制都实现了
5. 道具/代币已发布并等待生效

---

## 安全红线

⛔ **以下行为绝对禁止**：

1. **appKey 不得出现在前端代码、日志、公共仓库**
2. **不得跳过回调验证直接发货**
3. **不得以前端 success 回调作为最终交易依据**
4. **不得将 sessionKey 存储在客户端或日志中**

---

## 上线检查清单

| # | 检查项 | 完成 |
|---|--------|------|
| 1 | 将 `env` 从 `1` 改为 `0` | ☐ |
| 2 | 回调通知 URL 指向正式服务器 | ☐ |
| 3 | `appKey` 等密钥不在前端代码中暴露 | ☐ |
| 4 | 幂等处理已实现（防重复发货） | ☐ |
| 5 | 轮询 + 回调双保险机制已实现 | ☐ |
| 6 | 道具/代币配置已在后台**发布** | ☐ |

---

## AI 输出模板

AI 助手回复时使用以下结构：

```markdown
## 结论
[一句话说明当前是否可接入/可发布/已定位问题]

## 执行清单
- 已完成：
  - [item]
- 待完成：
  - [item]

## 风险与阻塞
- [风险或阻塞项]

## 下一步
1. [最小可执行动作]
2. [下一步验证动作]
```

---

## 🎉 恭喜！

如果你按照以上步骤操作，你的小程序虚拟支付就已经接入完成了。

### 快速回顾

```
开通虚拟支付 → 后台配置道具/代币 → 前端调 wx.requestVirtualPayment
→ 服务端算签名 → 处理回调发货 → 沙箱测试 → 上线
```

### 需要帮助？

- 📖 [微信官方文档](https://developers.weixin.qq.com/miniprogram/dev/platform-capabilities/business-capabilities/virtual-payment.html)
- 💬 [微信开放社区](https://developers.weixin.qq.com/community/develop/mixflow)
