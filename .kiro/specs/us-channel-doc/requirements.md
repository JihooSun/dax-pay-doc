# Requirements Document

> 需求文档：美国通道（tinylink）接入文档刷新

## Introduction

DCPAY 平台原有的商户接入文档（`dax-pay-doc/src/App.vue` 单页应用）完全基于印度场景编写：货币写死 INR、代付参数为印度银行账户体系（IFSC / IMPS / NEFT / RTGS / UPI / VPA）、回调以 UTR 流水号为主。

平台新增美国通道（tinylink）后，出现了一批印度文档未覆盖、且与印度差异明显的接入要点：

- 货币为 **USD**，金额仍精确到「分」。
- 代收为**收银台模式**：下单返回收银台 URL，用户在收银台内选择具体支付方式（CashApp / Apple Pay / Google Pay / PayPal），下单时商户无需指定支付方式。
- 代付**必须指定支付方式**：通过 `wayCode` + `wayParam` 传递，不再使用银行账户 / IFSC / IMPS / VPA 等印度字段；不同 `wayCode` 的 `wayParam` 字段不同；每个 `wayCode` 有独立的单笔限额与费率。
- 回调与查询的金额、货币语境随区域不同；UTR 为印度专属。

本次工作范围（已与项目所有人确认）：**新增一个独立的"美国通道（tinylink）接入"章节，集中说明美国区域（USD）的代收 / 代付 / 回调 / 错误码差异**，并在侧边栏导航新增对应入口。印度既有内容与章节保持不变。

### 关键决策（已确认）

1. **组织方式**：新增**独立章节**「美国通道（tinylink）接入」，集中承载美国区域的接入说明（代收、代付、回调、错误码差异）；侧边栏 `menuItems` **新增**对应导航条目。印度既有章节不动。
2. **不写具体费率数字**：文档不写死任何费率百分比 / 固定费（如 2% + $0.30），费率与限额统一表述为"以开户协议 / DCPAY 后台配置为准，详询对接群 / 管理员"。
3. **印度专属入参对美国不要求传**：美国代付不使用印度字段（银行账户 / IFSC / IMPS / VPA 等），美国章节只列美国实际需要的字段（`wayCode` / `wayParam` 等）；不要求美国商户传印度字段。
4. **wayParam 字段参考 tinylink 上游文档**：美国代付每个 `wayCode` 的 `wayParam` 必填字段，须与 tinylink 上游约定一致（见需求 3），但文档以 DCPAY 对外网关（`/unipay/*`）字段命名为准，不照搬 tinylink 自有接口字段（如 `mchOrderNo` / `timestamp` / `payOrderNo` 等）。
5. **代付方式全部展示 + 开启以对接群为准**：代付 `wayCode` 表完整列出 8 种（`ecashapp` / `paypal` / `venmo` / `emt` / `card` / `ach` / `chime` / `zelle`），并明确注明"具体为某商户开启哪些支付方式，以对接群 / 管理员确认为准"。

### 范围边界

- 仅修改文档工程 `dax-pay-doc`（`src/App.vue` 新增章节区块 + `menuItems` 新增导航条目）。
- 不改动后端、前端业务代码，不改动对外 API 行为；文档须如实描述**现有代码已实现**的行为。
- 文档语言为简体中文，与现有文档保持一致的版式（card / api-table / code-block）。
- 面向对象为**接入商户**，描述的是 DCPAY 对外统一网关（`/unipay/*`）的行为，不暴露 tinylink 上游通道的内部接口细节。

## Glossary

- **区域（region）**：商户所属国家，当前为 `india`（印度，INR）或 `usa`（美国，USD）。
- **wayCode**：支付方式编码，美国通道用于区分 CashApp / PayPal / Zelle / Card / ACH 等。
- **wayParam**：支付方式参数（JSON 字符串），随 `wayCode` 不同而字段不同。
- **收银台模式**：代收下单返回一个收银台页面 URL，由付款人在页面内完成支付方式选择与付款。

## Requirements

### 需求 1：新增独立的"美国通道（tinylink）接入"章节

**用户故事：** 作为一名美国区域接入商户，我希望有一个集中的"美国通道（tinylink）接入"章节，把美元代收 / 代付 / 回调 / 错误码差异一次讲清楚，以便我无需在印度章节里反复辨别哪些适用于我。

#### 验收标准

1. WHEN 文档加载 THEN 侧边栏 `menuItems` SHALL 新增一个指向"美国通道（tinylink）接入"章节的导航条目，且点击后锚点滚动定位正常工作。
2. WHEN 商户进入该章节 THEN 章节开头 SHALL 用一段总览说明：美国区域货币为 `USD`、通道为 tinylink、商户所属区域由 DCPAY 开户时分配且不可自行切换。
3. WHEN 商户阅读该章节总览 THEN 文档 SHALL 注明两区域**通用**的部分：统一网关地址、签名规则（RSA2）、订单查询接口、余额查询接口、金额单位均到「分」（这些仍以现有通用章节为准，美国章节只强调差异、不重复抄写）。
4. WHERE 美国章节内 THE 文档 SHALL 仅承载美国差异内容（代收差异、代付差异、回调差异、错误码补充），不复制粘贴印度专属字段说明。
5. WHEN 文档修改完成 THEN 现有印度相关章节与内容 SHALL 保持不变（不删除、不改变其语义）。

### 需求 2：美国章节内的代收下单说明

**用户故事：** 作为一名美国区域商户，我希望在美国章节里看到代收怎么发起：传什么货币、要不要指定支付方式、返回怎么用，以便正确发起一笔美元收款并把付款人引导到收银台。

#### 验收标准

1. WHEN 美国商户在美国章节查看代收说明 THEN 文档 SHALL 说明代收复用统一接口 `/unipay/pay`，请求参数与印度一致，仅 `currency` 取 `USD`，`amount` 精确到分。
2. WHEN 美国商户发起代收 THEN 文档 SHALL 说明代收为**收银台模式**：下单成功后返回收银台 URL（`payData`），商户须将付款人重定向到该 URL，由付款人在收银台内选择支付方式并完成付款。
3. WHERE 代收说明处 THE 文档 SHALL 明确：美国代收下单**无需**传 `wayCode`（付款人实际使用的支付方式由通道在回调中上报，下单时无需指定）。
4. WHEN 美国商户查看代收支持的支付方式 THEN 文档 SHALL 列出代收侧支持的支付方式名称（至少 CashApp、Apple Pay、Google Pay、PayPal），并说明费率以开户协议 / 对接群确认为准、不写具体数字。
5. IF 文档展示美国代收返回示例 THEN 示例 SHALL 体现收银台 URL（`payData` 非空、`deeplinks` 为空），与印度 deeplink 形态区分。
6. WHERE 美国代收返回字段说明 THE 文档 SHALL 注明 deeplink / UPI / GPay / PhonePe / Paytm 等字段不适用于美国（属印度形态）。

### 需求 3：美国章节内的代付下单说明（核心）

**用户故事：** 作为一名美国区域商户，我希望在美国章节里明确代付怎么用 `wayCode` + `wayParam` 指定收款方式、每种方式要传哪些参数，以便正确发起一笔美元代付且不被参数 / 限额校验拒绝。

#### 验收标准

1. WHEN 美国商户查看代付说明 THEN 文档 SHALL 说明代付复用统一接口 `/unipay/transfer`，`currency` 取 `USD`，并说明美国代付**不使用**印度银行字段（`bankName` / `branchName` / `bankAddress` / `accountNo` / `accountName` / `ifscCode` / `transferMode` / `vpa`），美国章节不要求传这些字段。
2. WHEN 美国商户发起代付 THEN 文档 SHALL 说明 `wayCode` 为**必传**、`wayParam` 为支付方式参数（JSON 对象），二者随支付方式不同；并给出美国代付请求参数表（含 `mchNo` / `bizOrderNo` / `amount` / `currency` / `wayCode` / `wayParam` / `reqTime` / `sign` 等以 DCPAY 网关命名的字段）。
3. WHEN 美国商户查看支持的代付方式 THEN 文档 SHALL 以表格**完整列出 8 种** `wayCode`、展示名、对应 `wayParam` 必填字段：`ecashapp`=cashtag、`paypal`=email、`venmo`=email、`emt`=email、`card`=cardNumber+cardValid、`ach`=accountNumber+routingNumber、`chime`=chimeSign、`zelle`=zelleSign；并明确注明"具体为某商户开启哪些支付方式，以对接群 / 管理员确认为准"。
4. WHEN 文档展示代付 `wayParam` 明细 THEN 文档 SHALL 按 `wayParam` 字段结构分组给出明细表（变量名、必填、示例、说明），分组与示例参考 tinylink 上游约定：
   - cashtag 类（`ecashapp`）：`{"cashtag":"$xxx"}`
   - email 类（`paypal` / `venmo` / `emt`）：`{"email":"xxx@xxx.com"}`
   - 银行卡类（`card`）：`{"cardNumber":"...","cardValid":"MM/YYYY"}`
   - ACH 类（`ach`）：`{"accountNumber":"...","routingNumber":"..."}`
   - chime 类（`chime`）：`{"chimeSign":"$xxx"}`
   - zelle 类（`zelle`）：`{"zelleSign":"xxx@xxx.com 或 +1xxxxxxxxxx"}`
   并说明 `wayParam` 必须为合法 JSON 对象、key 为合法 ASCII 标识符（DCPAY 网关会过滤非法 key）。
5. WHEN 文档展示美国代付请求示例 THEN 示例 SHALL 包含 `wayCode` 与 `wayParam`，**不**含印度银行字段；字段命名以 DCPAY 对外网关（`/unipay/transfer`）为准（如 `bizOrderNo`），**不**照搬 tinylink 自有字段（如 `mchOrderNo` / `timestamp` / `transferOrderNo`）。
6. WHERE 代付限额说明处 THE 文档 SHALL 说明：金额超出某 `wayCode` 的限额区间时下单会被拒绝（限额具体数值以对接群 / 后台为准、不写死），并指向对应错误码（见需求 5）。
7. WHERE 个别方式有到账特性 THE 文档 MAY 补充简短说明（如 ACH 周末亦可到账、到账时间较长），但不承诺具体时长。

### 需求 4：美国章节内的回调与查询说明

**用户故事：** 作为一名美国区域商户，我希望在美国章节了解回调里哪些字段有效、UTR 是否返回、金额货币如何理解，以便正确处理回调并对账。

#### 验收标准

1. WHEN 美国商户接收回调 THEN 文档 SHALL 说明回调复用统一机制：字段（`mchNo` / `bizOrderNo` / `orderNo` / `amount` / `status` / `sign` 等）、状态机、验签方式与印度一致（详见通用回调章节），美国章节只强调差异。
2. WHERE 回调 `utr` 字段说明处 THE 文档 SHALL 注明 `utr` 为印度代付专属，美国通道回调一般不返回 `utr`。
3. WHEN 美国商户处理回调金额 THEN 文档 SHALL 说明 `amount` 货币为 USD，金额口径与下单一致。
4. WHEN 美国商户查询订单或余额 THEN 文档 SHALL 说明查询 / 余额接口两区域通用，余额等金额字段单位为「分」，货币按美国为 USD 解释。

### 需求 5：美国章节内的错误码补充

**用户故事：** 作为一名美国区域商户，我希望代付涉及 wayCode 的错误能查到，以便快速定位下单被拒的原因。

#### 验收标准

1. WHEN 美国商户代付下单因支付方式问题被拒 THEN 文档 SHALL 覆盖以下情形：`wayCode` 缺失 / 为空、`wayCode` 未在通道启用、金额低于该 `wayCode` 最低限额、金额超过该 `wayCode` 最高限额、余额不足。
2. WHERE 错误项 THE 文档 SHALL 复用现有错误码体系（沿用既有 `code` 取值与「Insufficient balance」「amount limit」等既有条目），仅补充美国 wayCode 语境，不臆造后端未返回的错误码。
3. WHERE 错误码展示 THE 文档 MAY 在美国章节内以小表汇总上述与 wayCode 相关的错误情形，或指向现有错误码章节，二者择一保持简洁。

### 需求 6：版式一致性与导航

**用户故事：** 作为一名商户，我希望新增的美国章节能从侧边栏直达、与现有文档版式一致、阅读顺畅。

#### 验收标准

1. WHEN 文档加载 THEN 侧边栏 `menuItems` SHALL 新增"美国通道（tinylink）接入"对应条目，位置合理（建议置于代收 / 代付相关条目附近），点击锚点滚动定位正常工作。
2. WHEN 滚动到美国章节 THEN 现有的 `onScroll` 高亮逻辑 SHALL 能正确高亮该导航条目（章节根节点须带匹配的 `id` 与 `class="section"`）。
3. WHEN 新增任何内容 THEN 其 HTML 结构 SHALL 复用现有的 `card` / `api-table` / `code-block` / `content-header` 等既有样式类，不引入新的样式定义。
4. WHERE 美国章节内的区域提示 THE 文档 SHALL 使用统一、显眼的措辞（如「仅美国」「美国必传」「以对接群为准」），避免表述不一致。
5. WHEN 文档修改完成 THEN 现有印度相关章节与内容 SHALL 保持不变（不删除、不改变其语义）。

## 非目标（Out of Scope）

- 不拆分为印度版 / 美国版两套独立文档（仅在同一文档内新增一个美国章节）。
- 不写死任何费率数字（百分比 / 固定费）与限额具体数值，统一表述为"以开户协议 / 后台配置 / 对接群确认为准"。
- 不改动印度既有章节的内容与语义。
- 不改动后端、商户端、网关、收银台等业务代码。
- 不新增 tinylink 上游通道的内部协议说明（商户不直接对接 tinylink；文档以 DCPAY `/unipay/*` 网关字段为准）。
- 不照搬 tinylink 上游文档中与 DCPAY 网关无关的内容（如设备 ID/deviceId 获取方法、FingerprintJS 代码、card_ecommerce / card_direct 等当前未在 DCPAY 启用的复杂方式）。
- 不提供新的下载示例代码包（`dcpayDemo.zip`）；如需提及美国代付差异，仅作文字说明。
- 利润 / 费率的内部计算逻辑不在面向商户的文档中展开（商户只需知道自身承担的费率，且具体数字以对接群为准）。
