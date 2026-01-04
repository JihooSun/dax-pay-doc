<template>
  <div class="app">
    <header class="header">
      <div class="header-content">
        <div class="logo">
          <h1>D-Pay API 文档</h1>
        </div>
        <nav class="nav">
          <ul>
            <li><a href="#" @click="activeMenu = 'welcome'" :class="{ active: activeMenu === 'welcome' }">首页</a></li>
            <li><a href="#" @click="activeMenu = 'api-reference'" :class="{ active: activeMenu.startsWith('api-reference') }">API 参考</a></li>
            <li><a href="#" @click="activeMenu = 'quickstart'" :class="{ active: activeMenu.startsWith('quickstart') }">快速开始</a></li>
            <li><a href="#" @click="activeMenu = 'tutorials'" :class="{ active: activeMenu.startsWith('tutorials') }">教程</a></li>
            <li><a href="#" @click="activeMenu = 'support'" :class="{ active: activeMenu === 'support' }">支持</a></li>
          </ul>
        </nav>
      </div>
    </header>

    <main class="main">
      <aside class="sidebar">
        <div class="sidebar-content">
          <h3>导航菜单</h3>
          <ul>
            <li><a href="#" @click="activeMenu = 'welcome'" :class="{ active: activeMenu === 'welcome' }">欢迎</a></li>
            <li><a href="#" @click="activeMenu = 'overview'" :class="{ active: activeMenu === 'overview' }">概览</a></li>
            <li>
              <a href="#" @click="activeMenu = 'api-reference'" :class="{ active: activeMenu.startsWith('api-reference') }">API 参考</a>
              <ul class="submenu">
                <li><a href="#" @click="activeMenu = 'api-reference/auth'" :class="{ active: activeMenu === 'api-reference/auth' }">认证</a></li>
                <li><a href="#" @click="activeMenu = 'api-reference/payments'" :class="{ active: activeMenu === 'api-reference/payments' }">支付</a></li>
                <li><a href="#" @click="activeMenu = 'api-reference/refunds'" :class="{ active: activeMenu === 'api-reference/refunds' }">退款</a></li>
                <li><a href="#" @click="activeMenu = 'api-reference/reports'" :class="{ active: activeMenu === 'api-reference/reports' }">报告</a></li>
              </ul>
            </li>
            <li>
              <a href="#" @click="activeMenu = 'quickstart'" :class="{ active: activeMenu.startsWith('quickstart') }">快速开始</a>
              <ul class="submenu">
                <li><a href="#" @click="activeMenu = 'quickstart/setup'" :class="{ active: activeMenu === 'quickstart/setup' }">环境准备</a></li>
                <li><a href="#" @click="activeMenu = 'quickstart/first-request'" :class="{ active: activeMenu === 'quickstart/first-request' }">第一个请求</a></li>
                <li><a href="#" @click="activeMenu = 'quickstart/examples'" :class="{ active: activeMenu === 'quickstart/examples' }">示例代码</a></li>
              </ul>
            </li>
            <li>
              <a href="#" @click="activeMenu = 'tutorials'" :class="{ active: activeMenu.startsWith('tutorials') }">教程</a>
              <ul class="submenu">
                <li><a href="#" @click="activeMenu = 'tutorials/integration'" :class="{ active: activeMenu === 'tutorials/integration' }">集成指南</a></li>
                <li><a href="#" @click="activeMenu = 'tutorials/best-practices'" :class="{ active: activeMenu === 'tutorials/best-practices' }">最佳实践</a></li>
                <li><a href="#" @click="activeMenu = 'tutorials/troubleshooting'" :class="{ active: activeMenu === 'tutorials/troubleshooting' }">故障排除</a></li>
              </ul>
            </li>
          </ul>
        </div>
      </aside>

      <section class="content">
        <!-- 欢迎页面 -->
        <div v-if="activeMenu === 'welcome'">
          <div class="content-header">
            <h2>欢迎阅读 D-Pay API 文档</h2>
            <p>在这里，您将了解如何添加优质可靠的支付方式，从而拓展跨境业务。</p>
          </div>

          <div class="content-body">
            <div class="card">
              <h3>什么是 D-Pay？</h3>
              <p>D-Pay 是一个全面的支付基础设施平台，为企业提供安全、可靠的支付解决方案。我们的 API 允许您轻松集成各种支付方式，包括信用卡、借记卡、数字钱包等。</p>
              <p>无论您是初创企业还是大型企业，D-Pay 都能为您提供灵活的支付解决方案，帮助您拓展业务，提高支付处理效率。</p>
            </div>

            <div class="card">
              <h3>为什么选择 D-Pay？</h3>
              <div class="features">
                <div class="feature-item">
                  <h4>安全可靠</h4>
                  <p>PCI DSS 合规，采用银行级加密技术，确保您的交易数据安全。</p>
                </div>
                <div class="feature-item">
                  <h4>灵活扩展</h4>
                  <p>支持多种支付方式和货币，轻松拓展全球业务。</p>
                </div>
                <div class="feature-item">
                  <h4>易于集成</h4>
                  <p>提供详细的 API 文档和 SDK，快速集成到您的应用中。</p>
                </div>
                <div class="feature-item">
                  <h4>实时监控</h4>
                  <p>实时查看交易状态和报告，便于业务管理和决策。</p>
                </div>
              </div>
            </div>

            <div class="card">
              <h3>开始使用</h3>
              <ol>
                <li>创建一个 D-Pay 账户</li>
                <li>获取 API 密钥</li>
                <li>设置开发环境</li>
                <li>发送第一个 API 请求</li>
                <li>开始集成支付功能</li>
              </ol>
              <div style="margin-top: 16px;">
                <a href="#" @click="activeMenu = 'quickstart/setup'" class="btn">查看快速开始指南</a>
              </div>
            </div>
          </div>
        </div>

        <!-- 概览页面 -->
        <div v-else-if="activeMenu === 'overview'">
          <div class="content-header">
            <h2>D-Pay API 概览</h2>
            <p>了解 D-Pay API 的核心概念和架构设计。</p>
          </div>

          <div class="content-body">
            <div class="card">
              <h3>API 架构</h3>
              <p>D-Pay API 采用 RESTful 设计风格，使用标准的 HTTP 方法和状态码。API 以 JSON 格式返回数据，支持 UTF-8 编码。</p>
              <p>我们的 API 分为多个核心模块，每个模块负责特定的功能领域：</p>
              <ul>
                <li><strong>认证</strong> - 处理 API 密钥和认证流程</li>
                <li><strong>支付处理</strong> - 处理信用卡、借记卡等支付方式</li>
                <li><strong>退款管理</strong> - 处理退款请求和流程</li>
                <li><strong>报告生成</strong> - 生成交易报告和数据分析</li>
                <li><strong>风险管理</strong> - 提供欺诈检测和风险评估</li>
              </ul>
            </div>

            <div class="card">
              <h3>核心概念</h3>
              <div class="concepts">
                <div class="concept-item">
                  <h4>API 密钥</h4>
                  <p>用于认证 API 请求的密钥，包括公钥和私钥。</p>
                </div>
                <div class="concept-item">
                  <h4>支付方式</h4>
                  <p>指客户用于支付的方式，如信用卡、借记卡、数字钱包等。</p>
                </div>
                <div class="concept-item">
                  <h4>交易</h4>
                  <p>指一次完整的支付过程，包括授权、捕获、退款等步骤。</p>
                </div>
                <div class="concept-item">
                  <h4>商家</h4>
                  <p>指使用 D-Pay API 处理支付的企业或个人。</p>
                </div>
                <div class="concept-item">
                  <h4>订单</h4>
                  <p>指客户购买的商品或服务的记录。</p>
                </div>
              </div>
            </div>

            <div class="card">
              <h3>API 版本控制</h3>
              <p>D-Pay API 使用语义化版本控制，版本号格式为 vX.Y.Z，其中：</p>
              <ul>
                <li>X 表示主版本号，当进行不兼容的 API 更改时递增</li>
                <li>Y 表示次版本号，当添加新功能但保持向后兼容时递增</li>
                <li>Z 表示修订号，当进行向后兼容的错误修复时递增</li>
              </ul>
              <p>您可以在 API 请求中指定要使用的版本，例如：</p>
              <div class="code-block">
                <pre><code>GET /v2/payments HTTP/1.1
Host: api.finixpayment.com
Authorization: Bearer YOUR_API_KEY</code></pre>
              </div>
            </div>
          </div>
        </div>

        <!-- API 参考 - 认证 -->
        <div v-else-if="activeMenu === 'api-reference/auth'">
          <div class="content-header">
            <h2>API 认证</h2>
            <p>了解如何使用 API 密钥进行认证。</p>
          </div>

          <div class="content-body">
            <div class="card">
              <h3>获取 API 密钥</h3>
              <p>要使用 D-Pay API，您需要先获取 API 密钥。登录到 D-Pay 控制台，在 "API 密钥" 页面创建新的密钥对。</p>
              <p>每个 API 密钥对包含：</p>
              <ul>
                <li><strong>公钥</strong> - 用于客户端认证，如前端应用</li>
                <li><strong>私钥</strong> - 用于服务器端认证，必须保密</li>
              </ul>
            </div>

            <div class="card">
              <h3>认证方式</h3>
              <p>D-Pay API 支持以下认证方式：</p>
              <h4>Bearer Token</h4>
              <p>在请求头中添加 Authorization 字段，使用 Bearer 类型：</p>
              <div class="code-block">
                <pre><code>GET /v2/payments HTTP/1.1
Host: api.finixpayment.com
Authorization: Bearer YOUR_API_KEY</code></pre>
              </div>

              <h4>API 密钥对</h4>
              <p>使用公钥和私钥进行签名认证：</p>
              <div class="code-block">
                <pre><code>GET /v2/payments HTTP/1.1
Host: api.finixpayment.com
X-D-Pay-Public-Key: YOUR_PUBLIC_KEY
X-D-Pay-Signature: YOUR_SIGNATURE
X-D-Pay-Timestamp: 1634567890</code></pre>
              </div>
            </div>

            <div class="card">
              <h3>认证最佳实践</h3>
              <ul>
                <li>不要在客户端代码中暴露私钥</li>
                <li>定期轮换 API 密钥</li>
                <li>使用 HTTPS 加密所有 API 请求</li>
                <li>限制 API 密钥的访问权限</li>
                <li>监控 API 密钥的使用情况</li>
              </ul>
            </div>
          </div>
        </div>

        <!-- API 参考 - 支付 -->
        <div v-else-if="activeMenu === 'api-reference/payments'">
          <div class="content-header">
            <h2>支付 API</h2>
            <p>使用 D-Pay API 处理支付交易。</p>
          </div>

          <div class="content-body">
            <div class="card">
              <h3>创建支付</h3>
              <p>使用 POST 请求创建新的支付交易：</p>
              <div class="code-block">
                <pre><code>POST /v2/payments HTTP/1.1
Host: api.finixpayment.com
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "amount": 1000,
  "currency": "USD",
  "payment_method": "pm_1234567890",
  "description": "测试支付",
  "metadata": {
    "order_id": "ord_123456"
  }
}</code></pre>
              </div>

              <h4>请求参数</h4>
              <table class="api-table">
                <thead>
                  <tr>
                    <th>参数</th>
                    <th>类型</th>
                    <th>必填</th>
                    <th>描述</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>amount</td>
                    <td>integer</td>
                    <td>是</td>
                    <td>支付金额，单位为分</td>
                  </tr>
                  <tr>
                    <td>currency</td>
                    <td>string</td>
                    <td>是</td>
                    <td>货币代码，如 USD、CNY 等</td>
                  </tr>
                  <tr>
                    <td>payment_method</td>
                    <td>string</td>
                    <td>是</td>
                    <td>支付方式 ID</td>
                  </tr>
                  <tr>
                    <td>description</td>
                    <td>string</td>
                    <td>否</td>
                    <td>支付描述</td>
                  </tr>
                  <tr>
                    <td>metadata</td>
                    <td>object</td>
                    <td>否</td>
                    <td>附加元数据</td>
                  </tr>
                </tbody>
              </table>

              <h4>响应示例</h4>
              <div class="code-block">
                <pre><code>{
  "id": "pay_1234567890",
  "amount": 1000,
  "currency": "USD",
  "status": "succeeded",
  "payment_method": "pm_1234567890",
  "description": "测试支付",
  "created_at": "2026-01-05T12:00:00Z",
  "updated_at": "2026-01-05T12:00:00Z"
}</code></pre>
              </div>
            </div>

            <div class="card">
              <h3>支付状态</h3>
              <p>支付交易可以有以下状态：</p>
              <ul>
                <li><strong>pending</strong> - 支付处理中</li>
                <li><strong>succeeded</strong> - 支付成功</li>
                <li><strong>failed</strong> - 支付失败</li>
                <li><strong>cancelled</strong> - 支付已取消</li>
                <li><strong>refunded</strong> - 支付已退款</li>
              </ul>
            </div>
          </div>
        </div>

        <!-- API 参考 - 退款 -->
        <div v-else-if="activeMenu === 'api-reference/refunds'">
          <div class="content-header">
            <h2>退款 API</h2>
            <p>使用 D-Pay API 处理退款请求。</p>
          </div>

          <div class="content-body">
            <div class="card">
              <h3>创建退款</h3>
              <p>使用 POST 请求创建新的退款：</p>
              <div class="code-block">
                <pre><code>POST /v2/payments/{payment_id}/refunds HTTP/1.1
Host: api.finixpayment.com
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "amount": 500,
  "reason": "客户退货"
}</code></pre>
              </div>

              <h4>请求参数</h4>
              <table class="api-table">
                <thead>
                  <tr>
                    <th>参数</th>
                    <th>类型</th>
                    <th>必填</th>
                    <th>描述</th>
                  </tr>
                </thead>
                <tbody>
                  <tr>
                    <td>amount</td>
                    <td>integer</td>
                    <td>是</td>
                    <td>退款金额，单位为分</td>
                  </tr>
                  <tr>
                    <td>reason</td>
                    <td>string</td>
                    <td>否</td>
                    <td>退款原因</td>
                  </tr>
                </tbody>
              </table>
            </div>

            <div class="card">
              <h3>退款状态</h3>
              <p>退款可以有以下状态：</p>
              <ul>
                <li><strong>pending</strong> - 退款处理中</li>
                <li><strong>succeeded</strong> - 退款成功</li>
                <li><strong>failed</strong> - 退款失败</li>
              </ul>
            </div>
          </div>
        </div>

        <!-- API 参考 - 报告 -->
        <div v-else-if="activeMenu === 'api-reference/reports'">
          <div class="content-header">
            <h2>报告 API</h2>
            <p>生成和获取交易报告。</p>
          </div>

          <div class="content-body">
            <div class="card">
              <h3>生成报告</h3>
              <p>使用 POST 请求生成新的报告：</p>
              <div class="code-block">
                <pre><code>POST /v2/reports HTTP/1.1
Host: api.finixpayment.com
Authorization: Bearer YOUR_API_KEY
Content-Type: application/json

{
  "type": "daily_transactions",
  "start_date": "2026-01-01",
  "end_date": "2026-01-05"
}</code></pre>
              </div>

              <h4>报告类型</h4>
              <ul>
                <li><strong>daily_transactions</strong> - 每日交易报告</li>
                <li><strong>monthly_summary</strong> - 月度汇总报告</li>
                <li><strong>refund_summary</strong> - 退款汇总报告</li>
                <li><strong>chargeback_summary</strong> - 拒付汇总报告</li>
              </ul>
            </div>

            <div class="card">
              <h3>获取报告</h3>
              <p>使用 GET 请求获取报告详情：</p>
              <div class="code-block">
                <pre><code>GET /v2/reports/{report_id} HTTP/1.1
Host: api.finixpayment.com
Authorization: Bearer YOUR_API_KEY</code></pre>
              </div>
            </div>
          </div>
        </div>

        <!-- 快速开始 - 环境准备 -->
        <div v-else-if="activeMenu === 'quickstart/setup'">
          <div class="content-header">
            <h2>环境准备</h2>
            <p>设置开发环境，准备使用 D-Pay API。</p>
          </div>

          <div class="content-body">
            <div class="card">
              <h3>系统要求</h3>
              <p>使用 D-Pay API 前，确保您的开发环境满足以下要求：</p>
              <ul>
                <li>Node.js 14+ 或 Python 3.7+ 或其他支持 HTTP 请求的语言</li>
                <li>支持 HTTPS 的网络环境</li>
                <li>安装了适当的开发工具，如 IDE、终端等</li>
              </ul>
            </div>

            <div class="card">
              <h3>安装 SDK</h3>
              <p>D-Pay 提供了多种语言的 SDK，方便您快速集成 API：</p>
              
              <h4>Node.js SDK</h4>
              <div class="code-block">
                <pre><code>npm install finix</code></pre>
              </div>

              <h4>Python SDK</h4>
              <div class="code-block">
                <pre><code>pip install finix</code></pre>
              </div>

              <h4>Java SDK</h4>
              <div class="code-block">
                <pre><code>mvn install:install-file -Dfile=finix-java-sdk.jar -DgroupId=com.finix -DartifactId=finix-sdk -Dversion=1.0.0 -Dpackaging=jar</code></pre>
              </div>
            </div>

            <div class="card">
              <h3>配置环境变量</h3>
              <p>建议将 API 密钥存储在环境变量中，而不是硬编码在代码中：</p>
              
              <h4>Linux/macOS</h4>
              <div class="code-block">
                <pre><code>export FINIX_API_KEY=your_api_key
export FINIX_PUBLIC_KEY=your_public_key</code></pre>
              </div>

              <h4>Windows</h4>
              <div class="code-block">
                <pre><code>set FINIX_API_KEY=your_api_key
set FINIX_PUBLIC_KEY=your_public_key</code></pre>
              </div>
            </div>
          </div>
        </div>

        <!-- 快速开始 - 第一个请求 -->
        <div v-else-if="activeMenu === 'quickstart/first-request'">
          <div class="content-header">
            <h2>发送第一个请求</h2>
            <p>使用 D-Pay API 发送第一个请求，测试您的配置。</p>
          </div>

          <div class="content-body">
            <div class="card">
              <h3>测试连接</h3>
              <p>使用以下命令测试您的 API 连接：</p>
              
              <h4>使用 cURL</h4>
              <div class="code-block">
                <pre><code>curl -X GET https://api.finixpayment.com/v2/payments \
  -H "Authorization: Bearer YOUR_API_KEY"</code></pre>
              </div>

              <h4>使用 Node.js SDK</h4>
              <div class="code-block">
                <pre><code>const finix = require('finix')('YOUR_API_KEY', 'YOUR_PUBLIC_KEY');

async function testConnection() {
  try {
    const payments = await finix.payments.list();
    console.log('Connection successful!');
    console.log('Total payments:', payments.length);
  } catch (error) {
    console.error('Connection failed:', error.message);
  }
}

testConnection();</code></pre>
              </div>

              <h4>使用 Python SDK</h4>
              <div class="code-block">
                <pre><code>import finix

client = finix.D-PayClient('YOUR_API_KEY', 'YOUR_PUBLIC_KEY')

try:
    payments = client.payments.list()
    print('Connection successful!')
    print(f'Total payments: {len(payments)}')
except Exception as e:
    print(f'Connection failed: {e}')</code></pre>
              </div>
            </div>

            <div class="card">
              <h3>常见问题</h3>
              <p>如果您遇到连接问题，请检查以下几点：</p>
              <ul>
                <li>API 密钥是否正确</li>
                <li>网络连接是否正常</li>
                <li>防火墙是否阻止了请求</li>
                <li>请求格式是否正确</li>
              </ul>
            </div>
          </div>
        </div>

        <!-- 快速开始 - 示例代码 -->
        <div v-else-if="activeMenu === 'quickstart/examples'">
          <div class="content-header">
            <h2>示例代码</h2>
            <p>查看各种支付场景的示例代码。</p>
          </div>

          <div class="content-body">
            <div class="card">
              <h3>创建支付</h3>
              <p>使用信用卡创建支付：</p>
              <div class="code-block">
                <pre><code>const finix = require('finix')('YOUR_API_KEY', 'YOUR_PUBLIC_KEY');

async function createPayment() {
  try {
    // 创建支付方式
    const paymentMethod = await finix.paymentMethods.create({
      type: 'card',
      card: {
        number: '4242424242424242',
        exp_month: 12,
        exp_year: 2030,
        cvc: '123',
        name: '测试用户'
      }
    });

    // 创建支付
    const payment = await finix.payments.create({
      amount: 1000,
      currency: 'USD',
      payment_method: paymentMethod.id,
      description: '测试支付'
    });

    console.log('Payment created:', payment);
  } catch (error) {
    console.error('Error creating payment:', error);
  }
}

createPayment();</code></pre>
              </div>
            </div>

            <div class="card">
              <h3>处理退款</h3>
              <p>为支付创建退款：</p>
              <div class="code-block">
                <pre><code>const finix = require('finix')('YOUR_API_KEY', 'YOUR_PUBLIC_KEY');

async function createRefund() {
  try {
    const refund = await finix.refunds.create({
      payment_id: 'pay_1234567890',
      amount: 500,
      reason: '客户退货'
    });

    console.log('Refund created:', refund);
  } catch (error) {
    console.error('Error creating refund:', error);
  }
}

createRefund();</code></pre>
              </div>
            </div>
          </div>
        </div>

        <!-- 教程 - 集成指南 -->
        <div v-else-if="activeMenu === 'tutorials/integration'">
          <div class="content-header">
            <h2>集成指南</h2>
            <p>详细的支付集成步骤和最佳实践。</p>
          </div>

          <div class="content-body">
            <div class="card">
              <h3>前端集成</h3>
              <p>将 D-Pay 支付按钮集成到您的网站：</p>
              <ol>
                <li>在 HTML 页面中添加支付按钮：</li>
                <div class="code-block">
                  <pre><code>&lt;button id="finix-pay-button"&gt;立即支付&lt;/button&gt;</code></pre>
                </div>
                
                <li>引入 D-Pay JavaScript SDK：</li>
                <div class="code-block">
                  <pre><code>&lt;script src="https://js.finixpayment.com/v1/finix.js"&gt;&lt;/script&gt;</code></pre>
                </div>
                
                <li>初始化 SDK 并创建支付表单：</li>
                <div class="code-block">
                  <pre><code>&lt;script&gt;
                    const finix = new D-Pay('YOUR_PUBLIC_KEY');
                    
                    document.getElementById('finix-pay-button').addEventListener('click', async () => {
                      try {
                        const paymentMethod = await finix.paymentMethods.create({
                          type: 'card',
                          card: {
                            number: '4242424242424242',
                            exp_month: 12,
                            exp_year: 2030,
                            cvc: '123',
                            name: '测试用户'
                          }
                        });
                        
                        // 将 paymentMethod.id 发送到服务器
                        // 服务器使用此 ID 创建支付
                      } catch (error) {
                        console.error('Error:', error);
                      }
                    });
                  &lt;/script&gt;</code></pre>
                </div>
              </ol>
            </div>

            <div class="card">
              <h3>后端集成</h3>
              <p>在服务器端处理支付：</p>
              <div class="code-block">
                <pre><code>const express = require('express');
const finix = require('finix')('YOUR_API_KEY', 'YOUR_PUBLIC_KEY');
const app = express();

app.use(express.json());

app.post('/api/payments', async (req, res) => {
  try {
    const { payment_method_id, amount, currency } = req.body;
    
    const payment = await finix.payments.create({
      amount: amount,
      currency: currency,
      payment_method: payment_method_id,
      description: '网站支付'
    });
    
    res.json(payment);
  } catch (error) {
    res.status(500).json({ error: error.message });
  }
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});</code></pre>
              </div>
            </div>
          </div>
        </div>

        <!-- 教程 - 最佳实践 -->
        <div v-else-if="activeMenu === 'tutorials/best-practices'">
          <div class="content-header">
            <h2>最佳实践</h2>
            <p>遵循最佳实践，确保支付集成的安全性和可靠性。</p>
          </div>

          <div class="content-body">
            <div class="card">
              <h3>安全性最佳实践</h3>
              <ul>
                <li>不要在客户端代码中暴露 API 私钥</li>
                <li>使用 HTTPS 加密所有 API 请求</li>
                <li>定期轮换 API 密钥</li>
                <li>验证所有 API 响应的签名</li>
                <li>使用安全的存储方式保存敏感数据</li>
              </ul>
            </div>

            <div class="card">
              <h3>性能最佳实践</h3>
              <ul>
                <li>使用批量操作处理多个请求</li>
                <li>缓存频繁访问的数据</li>
                <li>优化 API 请求频率，避免过度请求</li>
                <li>使用异步处理长时间运行的操作</li>
              </ul>
            </div>

            <div class="card">
              <h3>错误处理最佳实践</h3>
              <ul>
                <li>捕获并处理所有 API 错误</li>
                <li>向用户显示友好的错误信息</li>
                <li>记录详细的错误日志，便于调试</li>
                <li>实现重试机制，处理临时错误</li>
              </ul>
            </div>
          </div>
        </div>

        <!-- 教程 - 故障排除 -->
        <div v-else-if="activeMenu === 'tutorials/troubleshooting'">
          <div class="content-header">
            <h2>故障排除</h2>
            <p>解决常见的支付集成问题。</p>
          </div>

          <div class="content-body">
            <div class="card">
              <h3>常见错误</h3>
              <p>以下是一些常见的错误和解决方案：</p>
              
              <h4>401 Unauthorized</h4>
              <p><strong>原因：</strong>API 密钥无效或认证失败</p>
              <p><strong>解决方案：</strong>检查 API 密钥是否正确，确保私钥未泄露</p>
              
              <h4>400 Bad Request</h4>
              <p><strong>原因：</strong>请求参数错误或格式不正确</p>
              <p><strong>解决方案：</strong>检查请求参数是否符合 API 文档要求，确保格式正确</p>
              
              <h4>500 Internal Server Error</h4>
              <p><strong>原因：</strong>D-Pay 服务器内部错误</p>
              <p><strong>解决方案：</strong>稍后重试，如问题持续，请联系 D-Pay 支持</p>
            </div>

            <div class="card">
              <h3>调试技巧</h3>
              <ul>
                <li>使用日志记录 API 请求和响应</li>
                <li>使用 D-Pay 控制台查看交易状态和错误信息</li>
                <li>测试不同的支付场景和边界情况</li>
                <li>使用调试工具分析网络请求</li>
              </ul>
            </div>
          </div>
        </div>

        <!-- 支持页面 -->
        <div v-else-if="activeMenu === 'support'">
          <div class="content-header">
            <h2>支持中心</h2>
            <p>获取帮助和支持。</p>
          </div>

          <div class="content-body">
            <div class="card">
              <h3>联系我们</h3>
              <p>如果您在使用 D-Pay API 时遇到问题，可以通过以下方式联系我们：</p>
              <ul>
                <li><strong>电子邮件：</strong>support@finixpayment.com</li>
                <li><strong>电话：</strong>+1 (555) 123-4567</li>
                <li><strong>在线聊天：</strong>在 D-Pay 控制台中使用在线聊天功能</li>
                <li><strong>工单系统：</strong>登录 D-Pay 控制台创建支持工单</li>
              </ul>
            </div>

            <div class="card">
              <h3>资源中心</h3>
              <p>访问我们的资源中心，获取更多帮助和信息：</p>
              <ul>
                <li><a href="#">API 文档</a> - 详细的 API 参考文档</li>
                <li><a href="#">开发者博客</a> - 最新的开发资讯和教程</li>
                <li><a href="#">常见问题</a> - 常见问题解答</li>
                <li><a href="#">视频教程</a> - 可视化的集成指南</li>
              </ul>
            </div>

            <div class="card">
              <h3>社区支持</h3>
              <p>加入我们的开发者社区，与其他开发者交流经验：</p>
              <ul>
                <li><strong>GitHub：</strong>https://github.com/finixpayment</li>
                <li><strong>Stack Overflow：</strong>使用 #finix 标签提问</li>
                <li><strong>开发者论坛：</strong>https://forum.finixpayment.com</li>
                <li><strong>社交媒体：</strong>关注 @D-PayPayment 获取最新动态</li>
              </ul>
            </div>
          </div>
        </div>
      </section>
    </main>

    <footer class="footer">
      <div class="footer-content">
        <p>&copy; 2026 D-Pay. All rights reserved.</p>
        <div class="footer-links">
          <a href="#" @click="activeMenu = 'privacy'">隐私政策</a>
          <a href="#" @click="activeMenu = 'terms'">服务条款</a>
          <a href="#" @click="activeMenu = 'contact'">联系我们</a>
        </div>
      </div>
    </footer>
  </div>
</template>

<script setup>
import { ref } from 'vue';

// 激活的菜单
const activeMenu = ref('welcome');
</script>

<style scoped>
/* 菜单激活状态样式 */
.nav a.active,
.sidebar a.active {
  color: #3498db;
  font-weight: 600;
  background-color: #f0f2f5;
}

/* 按钮样式 */
.btn {
  display: inline-block;
  padding: 10px 20px;
  background-color: #3498db;
  color: white;
  text-decoration: none;
  border-radius: 4px;
  transition: background-color 0.3s ease;
  font-weight: 500;
}

.btn:hover {
  background-color: #2980b9;
}

/* API 表格样式 */
.api-table {
  width: 100%;
  border-collapse: collapse;
  margin-top: 16px;
  margin-bottom: 16px;
}

.api-table th,
.api-table td {
  padding: 12px;
  text-align: left;
  border-bottom: 1px solid #eaecef;
}

.api-table th {
  background-color: #f8f9fa;
  font-weight: 600;
  color: #2c3e50;
}

.api-table tr:hover {
  background-color: #f8f9fa;
}

/* 概念项样式 */
.concepts {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
  gap: 16px;
  margin-top: 16px;
}

.concept-item {
  padding: 16px;
  background-color: #f8f9fa;
  border-radius: 6px;
  border: 1px solid #eaecef;
}

.concept-item h4 {
  margin-bottom: 8px;
  color: #3498db;
}
</style>