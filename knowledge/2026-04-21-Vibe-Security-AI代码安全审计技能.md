---
source: https://skills.sh/raroque/vibe-security-skill/vibe-security
tags: AI/Coding, 安全, Vibe Coding, 工具链
concepts:
- '[[concepts/Claude Code Skills]]'
related:
- '[[concepts/Claude Code Skills]]'
- '[[2026-04-21-Article-to-HTML-文章转信息图-Skill]]'
- '[[2026-04-23-Claude-多智能体协作5种模式]]'
title: Vibe Security — AI 代码安全审计技能
created: 2026-04-21
---
# Vibe Security — AI 代码安全审计技能

> Audit code for security vulnerabilities commonly introduced by AI code generation.

## 核心原则

**永远不信任客户端。** 每个价格、用户 ID、角色、订阅状态、特性开关和限流计数器都必须经过服务端验证或强制执行。如果它只存在于浏览器、移动端包或请求体中，攻击者就能控制它。

## 审计流程

对代码库进行系统性检查。对于每个步骤，**只有当代码库使用该技术时才加载相关参考文件**。跳过不相关的步骤。

### 9 大检查领域

| # | 领域 | 关键点 |
|---|------|--------|
| 1 | **Secrets & Env** | 硬编码密钥、NEXT_PUBLIC_/VITE_ 前缀、.gitignore |
| 2 | **Database Access Control** | Supabase RLS、Firebase Security Rules、Convex auth — vibe-coded app 第一大漏洞来源 |
| 3 | **Authentication & Authorization** | JWT 验证、中间件保护、Server Action、Session 管理 |
| 4 | **Rate Limiting** | 认证端、AI 调用、高开销操作必须限流 |
| 5 | **Payment Security** | 客户端价格篡改、Webhook 签名验证、订阅状态验证 |
| 6 | **Mobile Security** | 安全 Token 存储、API Key 后端代理、深链验证 |
| 7 | **AI/LLM Integration** | 暴露的 AI API Key、缺失用量上限、Prompt 注入、输出渲染安全 |
| 8 | **Deployment Configuration** | 生产设置、安全头、Source Map 暴露、环境隔离 |
| 9 | **Data Access & Input Validation** | SQL 注入、ORM 滥用、输入验证 |

## 核心指令

- 只报告真实安全问题，不挑剔风格或非安全相关问题
- 发现多个问题时，按可利用性和现实影响优先级排序
- 如果代码库未使用某技术（如无 Supabase），跳过该部分
- 生成新代码时，主动查阅相关参考文件避免引入漏洞
- **发现 Critical 漏洞（密钥泄露、RLS 禁用、认证绕过）立即置顶警告**

## 输出格式

按严重等级组织：Critical → High → Medium → Low

每个问题包含：
- 文件位置和相关行号
- 漏洞名称
- 攻击者能做什么（具体影响，不是抽象风险）
- Before/After 代码修复示例

### 输出示例

#### Critical

`lib/supabase.ts:3` — Supabase service_role key 暴露在客户端包

service_role key 可绕过所有 Row-Level Security。任何人都能从浏览器包中提取它，读写或删除整个数据库。

```ts
// ❌ Before
const supabase = createClient(url, process.env.NEXT_PUBLIC_SUPABASE_SERVICE_KEY!)

// ✅ After — 仅服务端使用 service_role，客户端用 anon key
const supabase = createClient(url, process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!)
```

#### High

`app/api/checkout/route.ts:15` — 价格从客户端请求体获取

攻击者可通过修改请求设置任意价格（包括 $0.01）。价格必须在服务端查询。

```ts
// ❌ Before
const session = await stripe.checkout.sessions.create({
  line_items: [{ price_data: { unit_amount: req.body.price } }]
})

// ✅ After — 服务端查询价格
const product = await db.products.findUnique({ where: { id: req.body.productId } })
const session = await stripe.checkout.sessions.create({
  line_items: [{ price: product.stripePriceId }]
})
```

## 在 Vibe Coding 工作流中的定位

- **代码生成前**（预防）+ **代码 review 时**（检测）双重使用
- AI 写代码快，但容易踩安全坑，此技能作为安全兜底

## 参考文件

- `references/secrets-and-env.md` — API 密钥、Token、环境变量配置、.gitignore 规则
- `references/database-security.md` — Supabase RLS、Firebase Security Rules、Convex auth 模式
- `references/authentication.md` — JWT 验证、中间件、Server Actions、Session 管理
- `references/rate-limiting.md` — 限流策略和滥用防护
- `references/payments.md` — Stripe 安全、Webhook 验证、价格验证
- `references/mobile.md` — React Native / Expo 安全：安全存储、API 代理、深链验证
- `references/ai-integration.md` — LLM API Key 保护、用量上限、Prompt 注入、输出清理
- `references/deployment.md` — 生产配置、安全头、环境分离
- `references/data-access.md` — SQL 注入防护、ORM 安全、输入验证
