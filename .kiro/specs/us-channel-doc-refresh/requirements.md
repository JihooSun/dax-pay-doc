# Requirements Document

## Introduction

DCPAY 的商户对接文档（`dax-pay-doc` 项目，单页 Vue 应用 `src/App.vue`）当前完全面向印度（India）场景编写，使用 INR 货币、银行转账字段（IFSC/IMPS/UPI 等）作为唯一示例。后端已新增美国支付通道 **tinylink**（USD 货币，按支付方式 `wayCode` 计费与限额），现有文档缺失该通道的对接说明。

本特性的目标是：在**保持现有单页结构和印度内容基本不变**的前提下，**仅新增**美国通道（tinylink）与印度通道的差异说明内容。差异内容覆盖代收下单、代付下单、回调通知、错误码等已有章节（通过子表格 / 标签页 / 或新增「美国通道(tinylink)接入」章节呈现），使美国商户能够正确对接代收与代付接口。

本特性**只交付 `src/App.vue` 的文档内容变更**，不修改后端代码、不重写印度内容、不拆分为两份文档。

## Glossary

- **Doc_Site**: 客户对接文档单页应用，对应 `dax-pay-doc/src/App.vue`，是本特性唯一需要修改的交付物。
- **India_Channel**: 现有印度支付通道场景，货币为 INR，代付使用银行账户字段（bankName/accountNo/ifscCode/transferMode/vpa 等）。
- **US_Channel**: 美国支付通道，通道编码为 `tinylink`，货币为 USD。
- **wayCode**: 支付方式编码，标识具体的美国支付方式（代收如 cashapp/applepay/googlepay/paypal；代付如 ecashapp/chime/zelle/paypal/card/ach）。
- **wayParam**: 代付请求中随 `wayCode` 提交的支付方式参数，为 JSON 字符串，不同 `wayCode` 的字段不同，最大 2048 位。
- **Collection_Section**: 文档中已有的「代收下单」章节（`id="pay"`）。
- **Transfer_Section**: 文档中已有的「代付下单」章节（`id="transfer"`）。
- **Notify_Section**: 文档中已有的「回调通知」章节（`id="notify"`）。
- **ErrorCode_Section**: 文档中已有的「错误码」章节（`id="errorcode"`）。
- **Single_Transaction_Limit**: 单笔交易限额，按 `wayCode` 维度配置的金额上下限（USD）。
- **Reader**: 阅读文档的对接开发者（商户技术人员）。

## Requirements

### Requirement 1: 货币（USD）差异说明

**User Story:** 作为美国商户的对接开发者，我想在文档中看到美国通道使用 USD 而非 INR 的明确说明，以便正确填写 `currency` 字段。

#### Acceptance Criteria

1. WHERE 代收下单章节或美国通道章节描述 `currency` 字段，THE Doc_Site SHALL 说明 US_Channel 的 `currency` 取值为 `USD`。
2. WHERE 代付下单章节或美国通道章节描述 `currency` 字段，THE Doc_Site SHALL 说明 US_Channel 的 `currency` 取值为 `USD`。
3. THE Doc_Site SHALL 保留 India_Channel 现有的 `currency` 取值 `INR` 的说明。
4. WHERE 文档展示 US_Channel 的请求示例，THE Doc_Site SHALL 在示例 JSON 中将 `currency` 字段值设为 `USD`。

### Requirement 2: 通道区分说明

**User Story:** 作为对接开发者，我想清楚区分印度通道与美国通道的差异，以便确认自己属于哪个场景并采用对应的字段与取值。

#### Acceptance Criteria

1. THE Doc_Site SHALL 提供 India_Channel 与 US_Channel 的区分说明，包含通道编码 `tinylink` 与货币 `USD` 标识。
2. THE Doc_Site SHALL 保留 India_Channel 现有文档内容，且现有印度内容文本保持基本不变。
3. THE Doc_Site SHALL 沿用现有单页结构与侧边栏导航，不拆分为多个独立文档页面。
4. WHERE US_Channel 差异内容被加入，THE Doc_Site SHALL 通过现有章节内的子表格、标签页，或新增「美国通道(tinylink)接入」章节呈现，且不删除已有的代收/代付/回调章节。

### Requirement 3: 代收下单 US 通道支付方式说明

**User Story:** 作为美国商户的对接开发者，我想了解 tinylink 代收支持的支付方式及其费率，以便预估成本并向用户展示。

#### Acceptance Criteria

1. THE Doc_Site SHALL 在代收差异内容中列出全部 4 个 US_Channel 代收 `wayCode`：`cashapp`、`applepay`、`googlepay`、`paypal`。
2. WHERE 文档列出代收 `wayCode` `cashapp`、`applepay`、`googlepay`，THE Doc_Site SHALL 标注其费率为 9% + $0.30/笔。
3. WHERE 文档列出代收 `wayCode` `paypal`，THE Doc_Site SHALL 标注其费率为 12.5% + $0.30/笔。
4. THE Doc_Site SHALL 说明 US_Channel 代收单笔限额为 $0.5 至 $5000。
5. THE Doc_Site SHALL 说明代收场景下 `wayCode` 由通道在回调时上报，商户下单时无需传入 `wayCode`。

### Requirement 4: 代付下单 US 通道新增字段说明

**User Story:** 作为美国商户的对接开发者，我想了解代付接口新增的 `wayCode` 与 `wayParam` 字段，以便正确构造美国通道的代付请求。

#### Acceptance Criteria

1. THE Doc_Site SHALL 在代付请求参数中说明 `wayCode` 字段，类型为 String，最大 32 位，含义为支付方式编码。
2. THE Doc_Site SHALL 说明 `wayCode` 字段在 US_Channel 代付场景下为必填，在 India_Channel 代付场景下为可选。
3. THE Doc_Site SHALL 在代付请求参数中说明 `wayParam` 字段，类型为 String（JSON 格式），最大 2048 位，含义为按 `wayCode` 不同而不同的支付方式参数。
4. THE Doc_Site SHALL 说明 India_Channel 专用的代付字段（`bankName`、`branchName`、`bankAddress`、`accountNo`、`accountName`、`ifscCode`、`number`、`transferMode`、`vpa`）不适用于 US_Channel。
5. THE Doc_Site SHALL 保留 India_Channel 现有代付字段的说明。

### Requirement 5: 代付 US 通道支付方式费率与限额表

**User Story:** 作为美国商户的对接开发者，我想看到每个代付支付方式的费率与单笔限额，以便选择合适的支付方式并避免超限报错。

#### Acceptance Criteria

1. THE Doc_Site SHALL 在代付差异内容中列出全部 6 个 US_Channel 代付 `wayCode`：`ecashapp`、`chime`、`zelle`、`paypal`、`card`、`ach`。
2. WHERE 文档列出代付 `wayCode` `ecashapp`，THE Doc_Site SHALL 标注费率 0% + $0、单笔限额 $10 至 $10000。
3. WHERE 文档列出代付 `wayCode` `chime`，THE Doc_Site SHALL 标注费率 0% + $0、单笔限额 $30 至 $10000。
4. WHERE 文档列出代付 `wayCode` `zelle`，THE Doc_Site SHALL 标注费率 0% + $0、单笔限额 $50 至 $5000。
5. WHERE 文档列出代付 `wayCode` `paypal`，THE Doc_Site SHALL 标注费率 2% + $0.30、单笔限额 $10 至 $2000。
6. WHERE 文档列出代付 `wayCode` `card`，THE Doc_Site SHALL 标注费率 3% + $0.50、单笔限额 $10 至 $1000。
7. WHERE 文档列出代付 `wayCode` `ach`，THE Doc_Site SHALL 标注费率 3% + $0.50、单笔限额 $10 至 $1000。
8. THE Doc_Site SHALL 说明代付单笔限额按 `wayCode` 在下单时校验，金额超出区间将下单失败。

### Requirement 6: 代付 wayParam JSON 示例

**User Story:** 作为美国商户的对接开发者，我想看到每个代付支付方式对应的 `wayParam` JSON 示例，以便填写正确的收款方信息。

#### Acceptance Criteria

1. THE Doc_Site SHALL 为代付场景提供至少一个完整的 US_Channel 代付请求示例，包含 `wayCode`、`wayParam` 与 `currency` 为 `USD`。
2. WHERE 文档展示 cashapp 类（`ecashapp`）`wayParam` 示例，THE Doc_Site SHALL 展示包含 `cashtag` 键的 JSON（例 `{"cashtag":"$xxx"}`）。
3. WHERE 文档展示基于邮箱的支付方式（如 `paypal`）`wayParam` 示例，THE Doc_Site SHALL 展示包含 `email` 键的 JSON。
4. WHERE 文档展示 `card` 支付方式 `wayParam` 示例，THE Doc_Site SHALL 展示包含卡号与有效期键的 JSON（例 `{"cardNumber":"...","cardValid":"..."}`）。
5. THE Doc_Site SHALL 说明 `wayParam` 的 JSON 字段随 `wayCode` 不同而不同。

### Requirement 7: 回调通知 US 通道差异说明

**User Story:** 作为美国商户的对接开发者，我想了解回调通知在美国通道下的差异，以便正确解析回调并完成对账。

#### Acceptance Criteria

1. THE Doc_Site SHALL 说明 `utr` 字段在 India_Channel 表示印度 UTR 流水号，在 US_Channel 表示通道订单参考号。
2. THE Doc_Site SHALL 保留回调通知现有的订单状态枚举说明，不更改其取值。
3. THE Doc_Site SHALL 保留回调通知现有的响应要求，即商户处理后必须返回 `SUCCESS`（不区分大小写）。
4. THE Doc_Site SHALL 保留回调通知现有的签名验证说明。

### Requirement 8: 错误码 US 通道差异说明

**User Story:** 作为美国商户的对接开发者，我想了解美国通道相关的错误码含义，以便处理超限与未启用支付方式等失败场景。

#### Acceptance Criteria

1. THE Doc_Site SHALL 说明错误码 `20060`（金额超过限额）在 US_Channel 场景下也覆盖按 `wayCode` 的单笔限额校验。
2. THE Doc_Site SHALL 说明当代付请求的 `wayCode` 未在 tinylink 通道启用时会返回错误（错误信息形如「代付方式 [xxx] 未在 tinylink 通道启用」）。
3. THE Doc_Site SHALL 保留现有错误码表中其余条目不变。

### Requirement 9: 内容准确性与完整性约束

**User Story:** 作为文档维护者，我想确保新增的美国通道内容与后端实现一致，以便商户依据文档能够成功对接。

#### Acceptance Criteria

1. THE Doc_Site SHALL 使每个 US_Channel 代付 `wayCode`（`ecashapp`、`chime`、`zelle`、`paypal`、`card`、`ach`）都对应展示其费率与单笔限额。
2. THE Doc_Site SHALL 使每个 US_Channel 代收 `wayCode`（`cashapp`、`applepay`、`googlepay`、`paypal`）都对应展示其费率。
3. WHERE 文档展示金额费率，THE Doc_Site SHALL 以美元（USD）为单位描述限额与固定手续费金额。
4. IF 新增的 US_Channel 内容与 India_Channel 内容并列展示，THEN THE Doc_Site SHALL 明确标注每段内容所属的通道，避免 Reader 混淆。
