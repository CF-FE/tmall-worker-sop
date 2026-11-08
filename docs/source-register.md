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
