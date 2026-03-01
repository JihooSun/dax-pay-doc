<script setup>
import { ref } from 'vue'

const activeMenu = ref('welcome')

const menuItems = [
  { key: 'welcome', title: '欢迎' },
  { key: 'sign', title: '签名规则' },
  { key: 'pay', title: '代收下单' },
  { key: 'transfer', title: '代付下单' },
  { key: 'query', title: '订单查询' },
  { key: 'telegram', title: 'Telegram机器人' },
]

function scrollToSection(key) {
  activeMenu.value = key
  const element = document.getElementById(key)
  if (element) {
    const headerHeight = 64 // header高度
    const elementPosition = element.getBoundingClientRect().top
    const offsetPosition = elementPosition + window.pageYOffset - headerHeight
    window.scrollTo({
      top: offsetPosition,
      behavior: 'smooth'
    })
  }
}
</script>

<template>
  <div class="app">
    <header class="header">
      <div class="header-content">
        <div class="logo">
          <img src="/blue_dcpay.png" alt="DCPAY" />
        </div>
        <nav class="nav">
          <ul>
            <li><a href="https://dcpay.me/" target="_blank">商户端登录</a></li>
            <li><a href="https://t.me/feileabc" target="_blank">技术支持</a></li>
          </ul>
        </nav>
      </div>
    </header>

    <main class="main">
      <aside class="sidebar">
        <div class="sidebar-content">
          <h3>API 文档</h3>
          <ul>
            <li v-for="item in menuItems" :key="item.key">
              <a 
                href="javascript:;" 
                :class="{ active: activeMenu === item.key }"
                @click="scrollToSection(item.key)"
              >
                {{ item.title }}
              </a>
            </li>
          </ul>
        </div>
      </aside>

      <section class="content">
        <!-- 欢迎 -->
        <div id="welcome" class="section">
          <div class="content-header">
            <h2>欢迎使用 DCPAY API</h2>
            <p>DCPAY 是一个安全、稳定、高效的支付服务平台，为您的业务提供全方位的支付解决方案。</p>
          </div>
          
          <div class="card">
            <h3>快速开始</h3>
            <p>通过 DCPAY API，您可以轻松集成代收（收款）和代付（转账）功能到您的应用中。</p>
            <div class="features">
              <div class="feature-item">
                <h4>🔐 安全可靠</h4>
                <p>采用 RSA2 签名验证机制，确保每一笔交易的安全性和完整性。</p>
              </div>
              <div class="feature-item">
                <h4>⚡ 快速集成</h4>
                <p>RESTful API 设计，支持 JSON 格式，轻松集成到任何开发环境。</p>
              </div>
              <div class="feature-item">
                <h4>📊 实时查询</h4>
                <p>支持订单状态实时查询，随时掌握交易进度。</p>
              </div>
              <div class="feature-item">
                <h4>🔄 代收代付</h4>
                <p>支持代收（收款）和代付（转账）两种交易模式，满足不同业务需求。</p>
              </div>
            </div>
          </div>

          <div class="card">
            <h3>接入流程</h3>
            <ol>
              <li><strong>注册商户</strong> - 联系运营人员注册商户账号，获取商户号（mchNo）</li>
              <li><strong>生成密钥</strong> - 生成 RSA 密钥对，将公钥配置到 DCPAY 后台</li>
              <li><strong>阅读文档</strong> - 熟悉 API 接口规范和签名规则</li>
              <li><strong>开发对接</strong> - 根据文档进行接口开发</li>
              <li><strong>测试验收</strong> - 在测试环境验证功能正确性</li>
              <li><strong>上线运营</strong> - 切换到生产环境，正式开始使用</li>
            </ol>
          </div>

          <div class="card">
            <h3>请求接口</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>项目</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>接口地址</td>
                  <td><code>https://api.dcpay.me</code></td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <!-- 签名规则 -->
        <div id="sign" class="section">
          <div class="content-header">
            <h2>签名规则</h2>
            <p>为保证接口调用的安全性，所有 API 请求都需要进行签名验证。DCPAY 采用 RSA2（SHA256WithRSA）签名算法。</p>
          </div>

          <div class="card">
            <h3>签名算法说明</h3>
            <p>DCPAY 使用 <strong>RSA2</strong>（SHA256WithRSA）签名算法，这是一种非对称加密算法：</p>
            <ul>
              <li><strong>商户端</strong>：使用 RSA 私钥对请求参数进行签名</li>
              <li><strong>DCPAY 服务端</strong>：使用商户配置的 RSA 公钥验证签名</li>
            </ul>
          </div>

          <div class="card">
            <h3>签名步骤</h3>
            <h4>第一步：参数排序</h4>
            <p>将请求参数按照以下规则进行处理：</p>
            <ol>
              <li>参数名按照 ASCII 码从小到大排序（字典序）</li>
              <li>参数值为空（null 或空字符串）不参与签名</li>
              <li>参数名不区分大小写</li>
              <li>排除 <code>sign</code> 字段本身</li>
            </ol>

            <h4>第二步：拼接字符串</h4>
            <p>将排序后的参数按照 <code>key1=value1&key2=value2&...</code> 格式拼接成字符串。</p>
            <div class="code-block">
              <pre><code>// 示例：待签名字符串
amount=100.00&bizOrderNo=ORDER_001&clientIp=127.0.0.1&mchNo=M123456&reqTime=1704067200000</code></pre>
            </div>

            <h4>第三步：生成签名</h4>
            <p>使用商户私钥对拼接后的字符串进行 <strong>RSA2（SHA256WithRSA）</strong> 签名：</p>
            <ol>
              <li>将待签名字符串转换为字节数组（UTF-8编码）</li>
              <li>使用商户私钥初始化签名对象，算法为 <code>SHA256WithRSA</code></li>
              <li>执行签名操作，得到签名字节数组</li>
              <li>将签名结果进行 Base64 编码，得到最终的签名字符串</li>
            </ol>
            <div class="code-block">
              <pre><code>// 签名结果示例（Base64编码）
// 原始待签名字符串：
// amount=100.00&bizOrderNo=ORDER_001&clientIp=127.0.0.1&mchNo=M123456&reqTime=1704067200000

// 最终签名值（Base64编码）：
// "K2Jx8vM3nQ7hR9sT..."</code></pre>
            </div>
          </div>

          <div class="card">
            <h3>密钥获取</h3>
            <p>密钥无需自行生成，请按以下步骤获取：</p>
            <ol>
              <li>登录 <a href="https://admin.dcpay.me/" target="_blank">商户端</a></li>
              <li>进入左侧菜单 <strong>设置</strong></li>
              <li>查看 <strong>商户私钥</strong> 与 <strong>平台公钥</strong></li>
            </ol>
            <div class="features" style="margin-top: 16px;">
              <div class="feature-item">
                <h4>商户私钥</h4>
                <p>用于调用代收下单、代付下单、订单查询接口时进行签名加密。</p>
              </div>
              <div class="feature-item">
                <h4>平台公钥</h4>
                <p>用于平台订单回调通知时进行签名验证解密。</p>
              </div>
            </div>
            <p style="margin-top: 16px;"><strong>注意：</strong>请妥善保管商户私钥，切勿泄露给他人。</p>
          </div>

          <div class="card">
            <h3>签名示例代码</h3>
            <h4>Java 示例</h4>
            <div class="code-block">
              <pre><code>import java.security.*;
import java.security.spec.*;
import java.util.*;
import java.nio.charset.StandardCharsets;
import java.util.Base64;

public class SignExample {
    
    /**
     * RSA2签名（SHA256WithRSA）
     * @param data 待签名字符串
     * @param privateKeyStr 私钥字符串（PEM格式，包含头尾标识）
     * @return Base64编码的签名字符串
     */
    public static String rsa2Sign(String data, String privateKeyStr) throws Exception {
        // 1. 处理私钥，去掉PEM头尾标识和空白字符
        String privateKeyPEM = privateKeyStr
            .replace("-----BEGIN PRIVATE KEY-----", "")
            .replace("-----END PRIVATE KEY-----", "")
            .replaceAll("\\s+", "");
        
        // 2. Base64解码私钥
        byte[] keyBytes = Base64.getDecoder().decode(privateKeyPEM);
        
        // 3. 构建PrivateKey对象
        PKCS8EncodedKeySpec keySpec = new PKCS8EncodedKeySpec(keyBytes);
        PrivateKey privateKey = KeyFactory.getInstance("RSA").generatePrivate(keySpec);
        
        // 4. 初始化签名对象，使用SHA256WithRSA算法
        Signature signature = Signature.getInstance("SHA256WithRSA");
        signature.initSign(privateKey);
        signature.update(data.getBytes(StandardCharsets.UTF_8));
        
        // 5. 执行签名并Base64编码
        byte[] signBytes = signature.sign();
        return Base64.getEncoder().encodeToString(signBytes);
    }
    
    /**
     * 构建待签名字符串
     * @param params 参数Map
     * @return 待签名字符串
     */
    public static String buildSignString(Map&lt;String, String&gt; params) {
        // 1. 按键名ASCII排序
        List&lt;String&gt; keys = new ArrayList&lt;&gt;(params.keySet());
        Collections.sort(keys);
        
        // 2. 拼接字符串
        StringBuilder sb = new StringBuilder();
        for (int i = 0; i &lt; keys.size(); i++) {
            String key = keys.get(i);
            String value = params.get(key);
            if (value != null &amp;&amp; !value.isEmpty() &amp;&amp; !"sign".equals(key)) {
                if (i > 0) sb.append("&amp;");
                sb.append(key).append("=").append(value);
            }
        }
        return sb.toString();
    }
    
    public static void main(String[] args) throws Exception {
        // 构建请求参数
        Map&lt;String, String&gt; params = new TreeMap&lt;&gt;();
        params.put("mchNo", "M123456");
        params.put("bizOrderNo", "ORDER_001");
        params.put("amount", "100.00");
        params.put("clientIp", "127.0.0.1");
        params.put("reqTime", String.valueOf(System.currentTimeMillis()));
        
        // 构建待签名字符串
        String signStr = buildSignString(params);
        System.out.println("待签名字符串: " + signStr);
        
        // 读取私钥（示例，实际应从文件读取）
        String privateKey = "-----BEGIN PRIVATE KEY-----\n" +
            "MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQC...\n" +
            "-----END PRIVATE KEY-----";
        
        // 生成签名
        String sign = rsa2Sign(signStr, privateKey);
        System.out.println("签名值: " + sign);
        
        // 将签名加入参数
        params.put("sign", sign);
    }
}</code></pre>
            </div>

            <h4>PHP 示例</h4>
            <div class="code-block">
              <pre><code>&lt;?php
/**
 * RSA2签名（SHA256WithRSA）
 * @param string $data 待签名字符串
 * @param string $privateKeyPem 私钥字符串（PEM格式）
 * @return string Base64编码的签名字符串
 */
function rsa2Sign($data, $privateKeyPem) {
    // 1. 加载私钥
    $privateKey = openssl_pkey_get_private($privateKeyPem);
    if (!$privateKey) {
        throw new Exception('私钥格式错误');
    }
    
    // 2. 使用SHA256WithRSA算法签名
    $signature = '';
    $result = openssl_sign($data, $signature, $privateKey, OPENSSL_ALGO_SHA256);
    
    if (!$result) {
        throw new Exception('签名失败: ' . openssl_error_string());
    }
    
    // 3. Base64编码并返回
    return base64_encode($signature);
}

/**
 * 构建待签名字符串
 * @param array $params 参数数组
 * @return string 待签名字符串
 */
function buildSignString($params) {
    // 1. 移除sign字段
    unset($params['sign']);
    
    // 2. 过滤空值
    $params = array_filter($params, function($v) {
        return $v !== null &amp;&amp; $v !== '';
    });
    
    // 3. 按键名ASCII排序
    ksort($params);
    
    // 4. 拼接字符串
    $pairs = [];
    foreach ($params as $key => $value) {
        $pairs[] = $key . '=' . $value;
    }
    
    return implode('&amp;', $pairs);
}

// ===== 使用示例 =====

// 构建请求参数
$params = [
    'mchNo' => 'M123456',
    'bizOrderNo' => 'ORDER_001',
    'amount' => '100.00',
    'clientIp' => '127.0.0.1',
    'reqTime' => round(microtime(true) * 1000)  // 13位时间戳
];

// 构建待签名字符串
$signStr = buildSignString($params);
echo "待签名字符串: " . $signStr . "\n";

// 商户私钥（从商户端设置页面获取）
$privateKeyPem = "-----BEGIN PRIVATE KEY-----
MIIEvgIBADANBgkqhkiG9w0BAQEFAASCBKgwggSkAgEAAoIBAQC...
-----END PRIVATE KEY-----";

// 生成签名
$sign = rsa2Sign($signStr, $privateKeyPem);
echo "签名值: " . $sign . "\n";

// 将签名加入参数
$params['sign'] = $sign;

// 发送请求
$jsonData = json_encode($params);
// 使用 curl 发送 POST 请求到 API...
?&gt;</code></pre>
            </div>
          </div>
        </div>

        <!-- 代收下单 -->
        <div id="pay" class="section">
          <div class="content-header">
            <h2>代收下单</h2>
            <p>代收（收款）接口，用于发起一笔收款订单。</p>
          </div>

          <div class="card">
            <h3>请求信息</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>项目</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>请求 URL</td>
                  <td><code>POST /unipay/pay</code></td>
                </tr>
                <tr>
                  <td>Content-Type</td>
                  <td><code>application/json</code></td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>请求参数</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>参数名</th>
                  <th>类型</th>
                  <th>必填</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>mchNo</code></td>
                  <td>String</td>
                  <td>是</td>
                  <td>商户号，由 DCPAY 分配</td>
                </tr>
                <tr>
                  <td><code>bizOrderNo</code></td>
                  <td>String</td>
                  <td>是</td>
                  <td>商户订单号，商户侧唯一标识，最大100位</td>
                </tr>
                <tr>
                  <td><code>amount</code></td>
                  <td>BigDecimal</td>
                  <td>是</td>
                  <td>支付金额，精度到分，最小0.01</td>
                </tr>
                <tr>
                  <td><code>currency</code></td>
                  <td>String</td>
                  <td>是</td>
                  <td>货币代码，如 INR（印度卢比）</td>
                </tr>
                <tr>
                  <td><code>title</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>支付标题，最大100位</td>
                </tr>
                <tr>
                  <td><code>clientIp</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>客户端 IP 地址</td>
                </tr>
                <tr>
                  <td><code>notifyUrl</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>异步通知地址，支付结果会通知到该地址</td>
                </tr>
                <tr>
                  <td><code>returnUrl</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>同步跳转地址，支付完成后跳转的页面</td>
                </tr>
                <tr>
                  <td><code>expiredTime</code></td>
                  <td>Long</td>
                  <td>否</td>
                  <td>订单过期时间，13位时间戳（毫秒）</td>
                </tr>
                <tr>
                  <td><code>extraParam</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>扩展参数，最大2048位</td>
                </tr>
                <tr>
                  <td><code>reqTime</code></td>
                  <td>Long</td>
                  <td>是</td>
                  <td>请求时间，13位时间戳（毫秒）</td>
                </tr>
                <tr>
                  <td><code>sign</code></td>
                  <td>String</td>
                  <td>是</td>
                  <td>签名值，使用 RSA2 签名</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>请求示例</h3>
            <div class="code-block">
              <pre><code>{
  "mchNo": "DC1010",
  "bizOrderNo": "ORDER_20260301001",
  "amount": 100.00,
  "currency": "INR",
  "title": "测试订单",
  "clientIp": "127.0.0.1",
  "notifyUrl": "https://example.com/notify",
  "returnUrl": "https://example.com/return",
  "reqTime": 1704067200000,
  "sign": "K2Jx8vM3nQ..."
}</code></pre>
            </div>
          </div>

          <div class="card">
            <h3>返回参数</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>参数名</th>
                  <th>类型</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>bizOrderNo</code></td>
                  <td>String</td>
                  <td>商户订单号</td>
                </tr>
                <tr>
                  <td><code>orderNo</code></td>
                  <td>String</td>
                  <td>DCPAY 订单号</td>
                </tr>
                <tr>
                  <td><code>status</code></td>
                  <td>String</td>
                  <td>支付状态：pending（待支付）、success（成功）、fail（失败）</td>
                </tr>
                <tr>
                  <td><code>payData</code></td>
                  <td>String</td>
                  <td>支付参数体，用于发起支付的参数</td>
                </tr>
                <tr>
                  <td><code>extraParam</code></td>
                  <td>String</td>
                  <td>支付扩展参数</td>
                </tr>
                <tr>
                  <td><code>amount</code></td>
                  <td>BigDecimal</td>
                  <td>支付金额</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>返回示例</h3>
            <div class="code-block">
              <pre><code>{
  "code": 0,
  "msg": "success",
  "data": {
    "bizOrderNo": "ORDER_20260301001",
    "orderNo": "DCP202603010001",
    "status": "pending",
    "payData": "https://pay.dcpay.com/...",
    "extraParam": null,
    "amount": 100.00
  }
}</code></pre>
            </div>
          </div>
        </div>

        <!-- 代付下单 -->
        <div id="transfer" class="section">
          <div class="content-header">
            <h2>代付下单</h2>
            <p>代付（转账）接口，用于向银行账户转账。</p>
          </div>

          <div class="card">
            <h3>请求信息</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>项目</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>请求 URL</td>
                  <td><code>POST /unipay/transfer</code></td>
                </tr>
                <tr>
                  <td>Content-Type</td>
                  <td><code>application/json</code></td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>请求参数</h3>
            <p>继承代收下单所有参数，额外增加以下参数：</p>
            <table class="api-table">
              <thead>
                <tr>
                  <th>参数名</th>
                  <th>类型</th>
                  <th>必填</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>bankName</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>银行名称，最大200位</td>
                </tr>
                <tr>
                  <td><code>branchName</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>支行名称，最大200位</td>
                </tr>
                <tr>
                  <td><code>bankAddress</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>银行地址，最大200位</td>
                </tr>
                <tr>
                  <td><code>accountNo</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>收款人账号（银行账号），最大50位</td>
                </tr>
                <tr>
                  <td><code>accountName</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>收款人姓名，最大50位</td>
                </tr>
                <tr>
                  <td><code>ifscCode</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>银行 IFSC 代码（印度银行），11位</td>
                </tr>
                <tr>
                  <td><code>number</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>用户手机号码，最大20位</td>
                </tr>
                <tr>
                  <td><code>transferMode</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>传输模式：IMPS、NEFT、RTGS、UPI</td>
                </tr>
                <tr>
                  <td><code>vpa</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>VPA 地址（UPI 支付），最大50位</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>请求示例</h3>
            <div class="code-block">
              <pre><code>{
  "mchNo": "DC1010",
  "bizOrderNo": "TRANSFER_20260301001",
  "amount": 1000.00,
  "currency": "INR",
  "title": "代付转账",
  "clientIp": "127.0.0.1",
  "notifyUrl": "https://example.com/transfer/notify",
  "bankName": "State Bank of India",
  "branchName": "Mumbai Branch",
  "accountNo": "1234567890123456",
  "accountName": "John Doe",
  "ifscCode": "SBIN0001234",
  "transferMode": "IMPS",
  "reqTime": 1704067200000,
  "sign": "K2Jx8vM3nQ..."
}</code></pre>
            </div>
          </div>

          <div class="card">
            <h3>返回参数</h3>
            <p>返回参数与代收下单相同，请参考代收下单返回参数说明。</p>
          </div>

          <div class="card">
            <h3>返回示例</h3>
            <div class="code-block">
              <pre><code>{
  "code": 0,
  "msg": "success",
  "data": {
    "bizOrderNo": "TRANSFER_20260301001",
    "orderNo": "DCDEV_T202603010001",
    "status": "pending",
    "payData": null,
    "extraParam": null,
    "amount": 1000.00
  }
}</code></pre>
            </div>
          </div>
        </div>

        <!-- 订单查询 -->
        <div id="query" class="section">
          <div class="content-header">
            <h2>订单查询</h2>
            <p>查询订单状态接口，可以查询代收订单或代付订单。</p>
          </div>

          <div class="card">
            <h3>请求信息</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>项目</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>请求 URL</td>
                  <td><code>POST /unipay/queryOrder</code></td>
                </tr>
                <tr>
                  <td>Content-Type</td>
                  <td><code>application/json</code></td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>请求参数</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>参数名</th>
                  <th>类型</th>
                  <th>必填</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>mchNo</code></td>
                  <td>String</td>
                  <td>是</td>
                  <td>商户号</td>
                </tr>
                <tr>
                  <td><code>queryType</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>查询类型：1-代收订单，2-代付订单</td>
                </tr>
                <tr>
                  <td><code>bizOrderNo</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>商户订单号，与 orderNo 二选一</td>
                </tr>
                <tr>
                  <td><code>orderNo</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>DCPAY 订单号，与 bizOrderNo 二选一</td>
                </tr>
                <tr>
                  <td><code>reqTime</code></td>
                  <td>Long</td>
                  <td>是</td>
                  <td>请求时间，13位时间戳（毫秒）</td>
                </tr>
                <tr>
                  <td><code>sign</code></td>
                  <td>String</td>
                  <td>是</td>
                  <td>签名值</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>请求示例</h3>
            <div class="code-block">
              <pre><code>{
  "mchNo": "DC1010",
  "queryType": "1",
  "bizOrderNo": "ORDER_20260301001",
  "reqTime": 1704067200000,
  "sign": "K2Jx8vM3nQ..."
}</code></pre>
            </div>
          </div>

          <div class="card">
            <h3>返回参数</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>参数名</th>
                  <th>类型</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>bizOrderNo</code></td>
                  <td>String</td>
                  <td>商户订单号</td>
                </tr>
                <tr>
                  <td><code>orderNo</code></td>
                  <td>String</td>
                  <td>DCPAY 订单号</td>
                </tr>
                <tr>
                  <td><code>status</code></td>
                  <td>String</td>
                  <td>订单状态</td>
                </tr>
                <tr>
                  <td><code>orderAmount</code></td>
                  <td>String</td>
                  <td>订单金额（分）</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>订单状态说明</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>状态值</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>pending</code></td>
                  <td>待处理</td>
                </tr>
                <tr>
                  <td><code>processing</code></td>
                  <td>处理中</td>
                </tr>
                <tr>
                  <td><code>success</code></td>
                  <td>成功</td>
                </tr>
                <tr>
                  <td><code>fail</code></td>
                  <td>失败</td>
                </tr>
                <tr>
                  <td><code>closed</code></td>
                  <td>已关闭</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>返回示例</h3>
            <div class="code-block">
              <pre><code>{
  "code": 0,
  "msg": "success",
  "data": {
    "bizOrderNo": "ORDER_20260301001",
    "orderNo": "DCP202603010001",
    "status": "success",
    "orderAmount": "10000"
  }
}</code></pre>
            </div>
          </div>
        </div>

        <!-- Telegram机器人 -->
        <div id="telegram" class="section">
          <div class="content-header">
            <h2>Telegram 机器人</h2>
            <p>DCPAY 提供 Telegram 机器人服务，方便商户实时查询账户信息和订单状态。</p>
          </div>

          <div class="card">
            <h3>机器人信息</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>项目</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td>机器人名称</td>
                  <td>DCPay小助手</td>
                </tr>
                <tr>
                  <td>机器人账号</td>
                  <td><a href="https://t.me/DCPaySupportBot" target="_blank">@DCPaySupportBot</a></td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>使用步骤</h3>
            <ol>
              <li>商户接入过程中，管理员会将 <code>@DCPaySupportBot</code> 拉入商户专属讨论组</li>
              <li>使用 <code>/bd 商户号</code> 命令绑定您的商户账号</li>
              <li>绑定成功后即可使用其他命令查询信息</li>
            </ol>
          </div>

          <div class="card">
            <h3>命令列表</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>命令</th>
                  <th>说明</th>
                  <th>示例</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>/bd 商户号</code></td>
                  <td>绑定商户账号，首次使用必须先绑定</td>
                  <td><code>/bd DC1010</code></td>
                </tr>
                <tr>
                  <td><code>ye</code></td>
                  <td>查询商户余额，显示可提现余额、待结算余额等信息</td>
                  <td><code>ye</code></td>
                </tr>
                <tr>
                  <td><code>success</code></td>
                  <td>查询商户成功率，显示不同时间段（5分钟、10分钟、30分钟、1小时、2小时）的代收和代付成功率</td>
                  <td><code>success</code></td>
                </tr>
                <tr>
                  <td><code>ds 订单号</code></td>
                  <td>查询代收订单详情，显示订单金额、状态、创建时间等信息</td>
                  <td><code>ds DCP202603010001</code></td>
                </tr>
                <tr>
                  <td><code>df 订单号</code></td>
                  <td>查询代付订单详情，显示订单金额、状态、创建时间等信息</td>
                  <td><code>df DCDEV_T202603010001</code></td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>命令详情</h3>
            
            <h4>绑定商户</h4>
            <p>首次使用机器人需要绑定商户账号，绑定后才能使用其他查询命令。</p>
            <div class="code-block">
              <pre><code>/bd DC1010</code></pre>
            </div>
            <p>绑定成功后会显示商户号和商户名称。</p>

            <h4>查询余额</h4>
            <p>查询当前绑定商户的余额信息，包括：</p>
            <ul>
              <li>可提现余额</li>
              <li>待结算余额</li>
              <li>代付冻结金额</li>
              <li>提现冻结金额</li>
            </ul>
            <div class="code-block">
              <pre><code>ye</code></pre>
            </div>

            <h4>查询成功率</h4>
            <p>查询不同时间段的代收和代付成功率，时间段包括：5分钟、10分钟、30分钟、1小时、2小时。</p>
            <div class="code-block">
              <pre><code>success</code></pre>
            </div>

            <h4>查询代收订单</h4>
            <p>根据订单号查询代收订单详情，显示订单金额、状态、创建时间、完成时间等信息。</p>
            <div class="code-block">
              <pre><code>ds DCP202603010001</code></pre>
            </div>

            <h4>查询代付订单</h4>
            <p>根据订单号查询代付订单详情，显示订单金额、状态、创建时间、完成时间等信息。</p>
            <div class="code-block">
              <pre><code>df DCDEV_T202603010001</code></pre>
            </div>
          </div>

          <div class="card">
            <h3>注意事项</h3>
            <ul>
              <li>每个 Telegram 账号只能绑定一个商户</li>
              <li>订单查询仅支持查询绑定商户的订单</li>
              <li>如有问题请联系技术支持：<a href="https://t.me/feileabc" target="_blank">@feileabc</a></li>
            </ul>
          </div>
        </div>
      </section>
    </main>
  </div>
</template>

<style src="./style.css"></style>
