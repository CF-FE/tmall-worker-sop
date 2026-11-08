# 资料索引与接口核查

整理日：2026-11-08。来源分层：官方文档说明机制，项目基线说明设计，教程说明作者经验，店铺实测才说明目标环境行为。以下所有目标店铺能力均未实测。

## 本轮直接复核的商品接口

| ID | 官方来源 | 文档确认内容 | 仍须验证 |
| --- | --- | --- | --- |
| E01 | [SKU 上下架管理](https://developer.alibaba.com/docs/api.htm?apiId=69204) | taobao.sku.update.listing.tmall，授权接口，SKU 状态映射 | 业务范围、发布体系、SKU 回读、并发与异步 |
| E02 | [商品编辑规则读取](https://developer.alibaba.com/docs/api.htm?apiId=53967) | alibaba.item.edit.schema.get，读取编辑规则 | 本商品实际 schema 与可改字段 |
| E03 | [商品编辑提交](https://developer.alibaba.com/docs/api.htm?apiId=53968) | alibaba.item.edit.submit，提交编辑后 schema | 实际授权、审核、生效、错误及部分失败 |
| E04 | [商品图片添加/更新](https://developer.alibaba.com/docs/api.htm?apiId=23) | taobao.item.img.upload，图片绑定和角色 | 适用发布体系、线上副作用、展示与回读 |

复核发现：E01 提示特殊发布页可能不支持，不能按接口名字承诺所有天猫商品适用。E02/E03 支持先取规则再提交，不应把文档样例的长度限制硬编码到全部商品。E04 的上传关联商品，不能一概视为不影响线上的素材暂存。

这些页面于本轮可读取其接口说明和参数。没有实际调用生产 API，没有取得目标账户开通证明。广告写接口、全量操作日志、条件写、严格幂等、实时展示和硬预算边界仍为 UNKNOWN。

## 本轮直接复核的工程机制

| ID | 来源 | 使用范围 | 不证明什么 |
| --- | --- | --- | --- |
| S01 | [OWASP Logging Cheat Sheet](https://cheatsheetseries.owasp.org/cheatsheets/Logging_Cheat_Sheet.html) | 日志上下文和敏感信息控制 | 日志多不等于动作安全；不自动产生授权 |
| S02 | [AWS：幂等 API 与安全重试](https://aws.amazon.com/builders-library/making-retries-safe-with-idempotent-APIs/) | 请求身份、迟到请求、参数不一致 | 不赋予天猫连接器服务端幂等 |
| S03 | [AWS：Transactional outbox](https://docs.aws.amazon.com/prescriptive-guidance/latest/cloud-design-patterns/transactional-outbox.html) | 本地状态和消息协调，消费去重 | 外部平台不在本地事务内 |
| S04 | [Azure：补偿事务](https://learn.microsoft.com/en-us/azure/architecture/patterns/compensating-transaction) | 并发与业务约束下的恢复 | 补偿不保证回到原状态或撤销历史消耗 |

复核日均为 2026-11-08。仓库的动作契约、工作流和验收项为项目综合设计，不是这些资料提供的现成天猫实现。

## 从 V0.5 继承的扩展阅读，尚未本轮逐页复核

下面保留可追查原始链接。其上一版记录的核查日期为 2026-10-08；该日期是旧稿陈述，不冒充本轮验证。

- [S3 Object Lock](https://docs.aws.amazon.com/AmazonS3/latest/userguide/object-lock.html)：版本保留、防篡改边界，不构成法律留存意见。
- [OpenTelemetry Logs Data Model](https://opentelemetry.io/docs/specs/otel/logs/data-model/)：发生与观察时间及追踪关联，不证明业务因果。
- [Azure Event Sourcing](https://learn.microsoft.com/en-us/azure/architecture/patterns/event-sourcing)：模式取舍，不要求全系统事件溯源。
- [阿里云百炼 OpenAI 兼容调用](https://help.aliyun.com/zh/model-studio/compatibility-of-openai-with-dashscope)：接入候选资料，地域、模型、价格与能力需当时复核。
- [Temporal Workflow Execution](https://docs.temporal.io/workflow-execution)：持久化编排候选，不代表已选型或部署。
- [Google Conversion Lift](https://support.google.com/google-ads/answer/12003020?hl=en)：增量测量参考，不外推天猫有相同功能。

## 资料进入 SOP 的规则

收集链接和简短原创概述 → 标注平台、时间与证据覆盖 → 对照官方规则 → 目标对象授权与实测 → 留存失败/干扰 → 评审是否进入 SOP。过期、矛盾和无法复核的内容保留限制或撤回，不用数量替代可信度。

## 七色米 ERP 与第三方 MCP 线索

详见 [七色米 ERP 接入能力研究](qisemi-erp-research.md)。该专题将官方模块 API 定制、电商同步及产品内 AI 介绍，与第三方 qisemierp MCP 清单明确分层。当前未确认官方 MCP、实际接口目录、鉴权、费用及天猫商品/广告写能力；帮助中心返回 403 的情况保留为访问缺口。没有安装或连接任何组件。

## V0.1 §21—22 历史原始来源

本轮已对照项目原稿核对以下11项名称、链接和使用边界；**未逐网页重新打开验证当前正文、可达性或更新时间**。原稿记载访问日为2026-10-07，以下历史日期来自该稿，不当作本轮事实复核。HR 编号对应旧稿 R 编号，以免与其他版本重号。

| 编号 | 历史来源 | 研究用途与限制 |
| --- | --- | --- |
| HR01 | [天猫2026 AI扶持商家](https://www.alibabanews.com/天貓2026加大投入用ai扶持商家增長/)（旧稿记2026-03-27） | 平台能力方向；不证明公开 API 或本店开通 |
| HR02 | [AI与天猫双11](https://www.alibabanews.com/ai首次全面落地天貓雙11創造實質價值/)（旧稿记2025-10-16） | 搜推、素材及分析趋势；宣传效果不作收益承诺 |
| HR03 | [千问与淘宝互通](https://www.alibabanews.com/千問app與淘寶全面互通開啟ai購物全新體驗/)（旧稿记2026-05-11） | 消费者侧购物方向；不能外推商家写权限 |
| HR04 | [天猫2025品牌策略](https://www.alibabanews.com/天貓2025全面投入扶持優質品牌增長/)（旧稿记2025-03-28） | 品牌、新品、复购研究；不是当前权益/资格说明 |
| HR05 | [淘宝天猫与小红书合作](https://www.alibabanews.com/淘寶天貓與小紅書深化戰略合作打造跨平台無縫購物體驗/)（旧稿记2025-05-07） | 站外至成交协同；具体产品与接口待查 |
| HR06 | [宝尊服务介绍](https://www.baozun.com/?lang=zh_CN) | 岗位/服务分工参照；不是统一岗位标准或本项目访谈 |
| HR07 | [QuanZhanTui机制论文](https://arxiv.org/abs/2507.02931) | 旧稿仅使用摘要；模型不等于当前广告规则。提交/接收日期本轮未复核 |
| HR08 | [Building effective agents](https://www.anthropic.com/engineering/building-effective-agents) | 简单工作流与自主 Agent 取舍；非电商效果验证 |
| HR09 | [LangGraph overview](https://docs.langchain.com/oss/python/langgraph/overview) | 候选状态编排；未完成本项目选型与压测 |
| HR10 | [百炼兼容接口](https://help.aliyun.com/zh/model-studio/compatibility-of-openai-with-dashscope) | 模型和地域差异；与上方继承阅读同一来源，不重复算独立证据 |
| HR11 | [LiteLLM Proxy Quick Start](https://docs.litellm.ai/docs/proxy/quick_start) | 模型网关候选；不代表生产安全与权限治理已完成 |

旧稿早期只读巡检优先的 MVP 已被后版范围收敛所调整，保留资料来源不表示恢复旧实施顺序。

## V0.4 继承商品接口线索

已对照 V0.4 原稿来源表核对，未在本轮重新读取以下官方网页或调用接口；保留为待复核线索，非本店已可调用清单。

| 编号 | 来源 | 范围与验证边界 |
| --- | --- | --- |
| E05 | [商品描述模块信息获取](https://developer.alibaba.com/docs/api.htm?apiId=22171)，tmall.item.desc.modules.get | 商详模块/约束的读取线索，不是商详发布；目标类目、账号读取权限与返回结构待验证 |
| E06 | [商品下架](https://developer.alibaba.com/docs/api.htm?apiId=53971)，alibaba.item.operate.downshelf | 整商品下架线索，不等于单 SKU 停售，也不证明会停广告；调用权限、适用市场、回读与异步行为待验证 |

生产使用前须取得对应权限并核对最新契约；读取能力不能扩展为写授权，下架是生产变更而非普通资料查询。
