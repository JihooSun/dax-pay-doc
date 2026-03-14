<script setup>
import { ref, onMounted, onUnmounted } from 'vue'

const activeMenu = ref('welcome')

let ticking = false

function onScroll() {
  if (ticking) return
  ticking = true
  requestAnimationFrame(() => {
    const headerHeight = 64
    const sections = document.querySelectorAll('.section')
    let current = sections[0]?.id || 'welcome'
    sections.forEach((section) => {
      const top = section.getBoundingClientRect().top
      if (top <= headerHeight + 10) {
        current = section.id
      }
    })
    activeMenu.value = current
    ticking = false
  })
}

onMounted(() => {
  window.addEventListener('scroll', onScroll, { passive: true })
})

onUnmounted(() => {
  window.removeEventListener('scroll', onScroll)
})

const menuItems = [
  { key: 'welcome', title: '欢迎' },
  { key: 'sign', title: '签名规则' },
  { key: 'pay', title: '代收下单' },
  { key: 'transfer', title: '代付下单' },
  { key: 'query', title: '订单查询' },
  { key: 'balance', title: '商户余额查询' },
  { key: 'notify', title: '回调通知' },
  { key: 'errorcode', title: '错误码' },
  { key: 'download', title: '示例代码' },
  { key: 'telegram', title: 'Telegram机器人' },
]

function scrollToSection(key) {
  activeMenu.value = key
  const element = document.getElementById(key)
  if (element) {
    const headerHeight = 64
    const elementPosition = element.getBoundingClientRect().top
    const offsetPosition = elementPosition + window.pageYOffset - headerHeight
    window.scrollTo({
      top: offsetPosition,
      behavior: 'smooth'
    })
  }
}

function downloadDemo() {
  const link = document.createElement('a')
  link.href = '/dcpayDemo.zip'
  link.download = 'dcpayDemo.zip'
  document.body.appendChild(link)
  link.click()
  document.body.removeChild(link)
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
amount=100&bizOrderNo=ORDER_001&clientIp=127.0.0.1&mchNo=M123456&reqTime=1704067200000</code></pre>
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
// amount=100&bizOrderNo=ORDER_001&clientIp=127.0.0.1&mchNo=M123456&reqTime=1704067200000

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
        params.put("amount", "100");
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
    'amount' => '100',
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
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>请求示例</h3>
            <div class="code-block">
              <pre><code>{
  "mchNo": "DC1010",
  "bizOrderNo": "ORDER_20260301001",
  "amount": 100,
  "currency": "INR",
  "reqTime": 1704067200000,
  "sign": "K2Jx8vM3nQ...",
  "title": "测试订单",
  "clientIp": "127.0.0.1",
  "notifyUrl": "https://example.com/notify",
  "returnUrl": "https://example.com/return"
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
    "amount": "100"
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
  "amount": 1000,
  "currency": "INR",
  "reqTime": 1704067200000,
  "sign": "K2Jx8vM3nQ...",
  "title": "代付转账",
  "clientIp": "127.0.0.1",
  "notifyUrl": "https://example.com/transfer/notify",
  "bankName": "State Bank of India",
  "branchName": "Mumbai Branch",
  "accountNo": "1234567890123456",
  "accountName": "John Doe",
  "ifscCode": "SBIN0001234",
  "transferMode": "IMPS"
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
    "amount": "1000"
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
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>请求示例</h3>
            <div class="code-block">
              <pre><code>{
  "mchNo": "DC1010",
  "reqTime": 1704067200000,
  "sign": "K2Jx8vM3nQ...",
  "queryType": "1",
  "bizOrderNo": "ORDER_20260301001"
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

        <!-- 商户余额查询 -->
        <div id="balance" class="section">
          <div class="content-header">
            <h2>商户余额查询</h2>
            <p>查询商户账户余额信息接口，可查询可用余额、待结算余额、冻结余额等。</p>
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
                  <td><code>POST /unipay/queryBalance</code></td>
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
                <tr>
                  <td><code>clientIp</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>客户端 IP 地址</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>请求示例</h3>
            <div class="code-block">
              <pre><code>{
  "mchNo": "DC1010",
  "reqTime": 1704067200000,
  "sign": "K2Jx8vM3nQ...",
  "clientIp": "127.0.0.1"
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
                  <td><code>withdrawableAmount</code></td>
                  <td>Long</td>
                  <td>可提现余额（分）</td>
                </tr>
                <tr>
                  <td><code>pendingAmount</code></td>
                  <td>Long</td>
                  <td>待结算余额（分）</td>
                </tr>
                <tr>
                  <td><code>payoutFrozenAmount</code></td>
                  <td>Long</td>
                  <td>代付冻结余额（分）</td>
                </tr>
                <tr>
                  <td><code>withdrawFrozenAmount</code></td>
                  <td>Long</td>
                  <td>提现冻结余额（分）</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>余额类型说明</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>余额类型</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>可提现余额</code></td>
                  <td>商户可申请提现的余额，已结算完成</td>
                </tr>
                <tr>
                  <td><code>待结算余额</code></td>
                  <td>订单已完成但尚未结算的余额，需等待结算周期</td>
                </tr>
                <tr>
                  <td><code>代付冻结余额</code></td>
                  <td>代付订单处理中冻结的余额，订单完成后解冻</td>
                </tr>
                <tr>
                  <td><code>提现冻结余额</code></td>
                  <td>提现申请处理中冻结的余额，提现完成后扣除</td>
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
    "withdrawableAmount": "1000000",
    "pendingAmount": "500000",
    "payoutFrozenAmount": "200000",
    "withdrawFrozenAmount": "100000"
  }
}</code></pre>
            </div>
            <p style="margin-top: 12px;"><strong>说明：</strong>以上金额单位为分，例如 <code>withdrawableAmount: 1000000</code> 表示可提现余额为 10000.00 元。</p>
          </div>
        </div>
        <!-- 回调通知 -->
        <div id="notify" class="section">
          <div class="content-header">
            <h2>回调通知</h2>
            <p>当订单状态发生变化（支付成功、代付完成等）时，DCPAY 会向下单时填写的 <code>notifyUrl</code> 发送 GET 请求，通知商户处理业务逻辑。</p>
          </div>

          <div class="card">
            <h3>通知说明</h3>
            <ul>
              <li>通知方式：<strong>HTTP GET</strong>，参数拼接在 URL 上</li>
              <li>触发时机：订单状态变更时（如支付成功、代付完成）</li>
              <li>重试机制：若商户未返回 <code>SUCCESS</code>，系统将按梯度间隔重试，最多重试 16 次</li>
              <li>商户必须在 5 秒内响应，超时视为失败</li>
            </ul>
          </div>

          <div class="card">
            <h3>回调参数</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>参数名</th>
                  <th>类型</th>
                  <th>必有</th>
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
                  <td><code>bizOrderNo</code></td>
                  <td>String</td>
                  <td>是</td>
                  <td>商户订单号</td>
                </tr>
                <tr>
                  <td><code>orderNo</code></td>
                  <td>String</td>
                  <td>是</td>
                  <td>DCPAY 订单号</td>
                </tr>
                <tr>
                  <td><code>amount</code></td>
                  <td>BigDecimal</td>
                  <td>是</td>
                  <td>订单金额</td>
                </tr>
                <tr>
                  <td><code>status</code></td>
                  <td>String</td>
                  <td>是</td>
                  <td>订单状态（见下方状态说明）</td>
                </tr>
                <tr>
                  <td><code>sign</code></td>
                  <td>String</td>
                  <td>是</td>
                  <td>签名值，使用平台公钥验签</td>
                </tr>
                <tr>
                  <td><code>title</code></td>
                  <td>String</td>
                  <td>否</td>
                  <td>订单标题，下单时选填，未填则不返回</td>
                </tr>
                <tr>
                  <td><code>payTime</code></td>
                  <td>Long</td>
                  <td>否</td>
                  <td>支付/完成时间，13位时间戳（毫秒），订单未完成时不返回</td>
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
            <h3>签名验证</h3>
            <p>收到回调后，建议验证签名以确保通知来自 DCPAY，防止伪造请求。</p>
            <ol>
              <li>取出所有参数，去掉 <code>sign</code> 字段</li>
              <li>过滤空值参数</li>
              <li>按参数名 ASCII 升序排列，拼接为 <code>key=value&amp;key=value</code> 格式</li>
              <li>使用<strong>平台公钥</strong>（从商户端设置页面获取）对签名字符串进行 RSA2 验签</li>
            </ol>
            <p>平台公钥请登录 <a href="https://admin.dcpay.me/" target="_blank">商户端</a> → 设置 页面获取。</p>
          </div>

          <div class="card">
            <h3>响应要求</h3>
            <p>商户处理完业务逻辑后，必须返回字符串 <code>SUCCESS</code>（不区分大小写），否则 DCPAY 会认为通知失败并重试。</p>
            <div class="code-block">
              <pre><code>SUCCESS</code></pre>
            </div>
          </div>

          <div class="card">
            <h3>回调示例（Java）</h3>
            <div class="code-block">
              <pre><code>import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RequestParam;
import org.springframework.web.bind.annotation.RestController;

import java.security.*;
import java.security.spec.*;
import java.util.*;
import java.nio.charset.StandardCharsets;

@RestController
@RequestMapping("/notify")
public class NotifyController {

    // 平台公钥（从商户端设置页面获取）
    private static final String PLATFORM_PUBLIC_KEY = "-----BEGIN PUBLIC KEY-----\n" +
        "您的平台公钥内容...\n" +
        "-----END PUBLIC KEY-----";

    @GetMapping
    public String notify(@RequestParam Map&lt;String, String&gt; params) {
        try {
            String sign       = params.get("sign");
            String mchNo      = params.get("mchNo");      // 必有
            String bizOrderNo = params.get("bizOrderNo"); // 必有
            String orderNo    = params.get("orderNo");    // 必有
            String status     = params.get("status");     // 必有
            String amount     = params.get("amount");     // 必有
            String title      = params.get("title");      // 可能为空
            String payTime    = params.get("payTime");    // 可能为空

            // 1. 验证签名
            if (sign == null || !verifySign(params, sign, PLATFORM_PUBLIC_KEY)) {
                return "FAIL";
            }

            // 2. 处理业务逻辑
            // 回调中无 noticeType，可通过本地数据库查询 bizOrderNo 对应的订单类型来区分代收/代付
            if ("success".equals(status)) {
                // TODO: 更新本地订单状态为成功，执行后续业务（发货、结算等）
                System.out.println("订单成功: " + bizOrderNo + "，金额: " + amount);
            } else if ("fail".equals(status)) {
                // TODO: 更新本地订单状态为失败
                System.out.println("订单失败: " + bizOrderNo);
            } else if ("closed".equals(status)) {
                // TODO: 更新本地订单状态为已关闭
                System.out.println("订单已关闭: " + bizOrderNo);
            }

            // 3. 返回 SUCCESS 告知 DCPAY 通知已处理，停止重试
            return "SUCCESS";
        } catch (Exception e) {
            e.printStackTrace();
            return "FAIL";
        }
    }

    /**
     * RSA2 验签（SHA256WithRSA）
     * 与服务端 PaySignUtil.createLinkString 保持一致：去掉 " 和 \ 字符
     */
    private boolean verifySign(Map&lt;String, String&gt; params, String sign, String publicKeyStr) throws Exception {
        List&lt;String&gt; keys = new ArrayList&lt;&gt;(params.keySet());
        Collections.sort(keys);
        StringBuilder sb = new StringBuilder();
        for (String key : keys) {
            String value = params.get(key);
            if (!"sign".equals(key) &amp;&amp; value != null &amp;&amp; !value.isEmpty()) {
                if (sb.length() &gt; 0) sb.append("&amp;");
                sb.append(key).append("=").append(value);
            }
        }
        // 去掉 " 和 \ 字符，与服务端签名逻辑保持一致
        String signStr = sb.toString().replace("\\", "").replace("\"", "");

        String pubKeyPEM = publicKeyStr
            .replace("-----BEGIN PUBLIC KEY-----", "")
            .replace("-----END PUBLIC KEY-----", "")
            .replaceAll("\\s+", "");
        byte[] keyBytes = Base64.getDecoder().decode(pubKeyPEM);
        PublicKey publicKey = KeyFactory.getInstance("RSA")
            .generatePublic(new X509EncodedKeySpec(keyBytes));

        Signature signature = Signature.getInstance("SHA256WithRSA");
        signature.initVerify(publicKey);
        signature.update(signStr.getBytes(StandardCharsets.UTF_8));
        return signature.verify(Base64.getDecoder().decode(sign));
    }
}</code></pre>
            </div>
          </div>

          <div class="card">
            <h3>回调示例（PHP）</h3>
            <div class="code-block">
              <pre><code>&lt;?php
// 平台公钥（从商户端设置页面获取）
$platformPublicKey = "-----BEGIN PUBLIC KEY-----
您的平台公钥内容...
-----END PUBLIC KEY-----";

// 1. 获取所有回调参数
$params = $_GET;
$sign = $params['sign'] ?? '';

// 2. 验证签名
if (empty($sign) || !verifySign($params, $sign, $platformPublicKey)) {
    echo 'FAIL';
    exit;
}

// 3. 处理业务逻辑
// 回调中无 noticeType，可通过本地数据库查询 bizOrderNo 对应的订单类型来区分代收/代付
$bizOrderNo = $params['bizOrderNo'] ?? ''; // 必有
$orderNo    = $params['orderNo'] ?? '';    // 必有
$status     = $params['status'] ?? '';     // 必有
$amount     = $params['amount'] ?? '';     // 必有
$title      = $params['title'] ?? '';      // 可能为空
$payTime    = $params['payTime'] ?? '';    // 可能为空

if ($status === 'success') {
    // TODO: 更新本地订单状态为成功，执行后续业务（发货、结算等）
    error_log("订单成功: " . $bizOrderNo . "，金额: " . $amount);
} elseif ($status === 'fail') {
    // TODO: 更新本地订单状态为失败
    error_log("订单失败: " . $bizOrderNo);
} elseif ($status === 'closed') {
    // TODO: 更新本地订单状态为已关闭
    error_log("订单已关闭: " . $bizOrderNo);
}

// 4. 返回 SUCCESS
echo 'SUCCESS';

/**
 * RSA2 验签（SHA256WithRSA）
 * 与服务端 PaySignUtil.createLinkString 保持一致：去掉 " 和 \ 字符
 */
function verifySign($params, $sign, $publicKeyPem) {
    unset($params['sign']);
    // 过滤空值
    $params = array_filter($params, function($v) { return $v !== null &amp;&amp; $v !== ''; });
    // 按参数名 ASCII 升序排列
    ksort($params);
    $pairs = [];
    foreach ($params as $k =&gt; $v) {
        $pairs[] = $k . '=' . $v;
    }
    // 去掉 " 和 \ 字符，与服务端签名逻辑保持一致
    $signStr = str_replace(['\\', '"'], '', implode('&amp;', $pairs));

    $publicKey = openssl_pkey_get_public($publicKeyPem);
    $result = openssl_verify($signStr, base64_decode($sign), $publicKey, OPENSSL_ALGO_SHA256);
    return $result === 1;
}
?&gt;</code></pre>
            </div>
          </div>
        </div>

        <!-- 错误码 -->
        <div id="errorcode" class="section">
          <div class="content-header">
            <h2>错误码</h2>
            <p>API 接口返回的错误码及错误信息说明，方便商户进行错误处理。</p>
          </div>

          <div class="card">
            <h3>错误码规范</h3>
            <p>API 返回格式：</p>
            <div class="code-block">
              <pre><code>{
  "code": 20011,
  "msg": "Merchant payment channel not configured"
}</code></pre>
            </div>
            <p style="margin-top: 12px;"><strong>说明：</strong><code>code</code> 为 0 表示成功，非 0 表示失败。失败时 <code>msg</code> 会返回具体的错误信息。</p>
          </div>

          <div class="card">
            <h3>代收下单错误码</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th style="width: 100px;">错误码</th>
                  <th style="width: 320px;">错误信息</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>20011</code></td>
                  <td>Merchant payment channel not configured</td>
                  <td>商户未配置支付通道，请联系运营人员配置</td>
                </tr>
                <tr>
                  <td><code>20011</code></td>
                  <td>Payment channel not enabled: xxx</td>
                  <td>支付通道未启用，请联系运营人员开启</td>
                </tr>
                <tr>
                  <td><code>20043</code></td>
                  <td>Payment is in progress, please do not repeat</td>
                  <td>支付处理中，请勿重复提交</td>
                </tr>
                <tr>
                  <td><code>20044</code></td>
                  <td>Payment timeout, please confirm payment status</td>
                  <td>支付已超时，请重新确认支付状态</td>
                </tr>
                <tr>
                  <td><code>20044</code></td>
                  <td>Order number duplicate, please initiate payment again</td>
                  <td>订单号重复，请重新发起支付</td>
                </tr>
                <tr>
                  <td><code>20044</code></td>
                  <td>Payment already successful, please do not pay again</td>
                  <td>已经支付成功，请勿重复支付</td>
                </tr>
                <tr>
                  <td><code>20044</code></td>
                  <td>Order payment failed or has been closed</td>
                  <td>该订单支付失败或已被关闭</td>
                </tr>
                <tr>
                  <td><code>20044</code></td>
                  <td>Order is not in pending payment status, please confirm order status</td>
                  <td>订单不是待支付状态，请确认订单状态</td>
                </tr>
                <tr>
                  <td><code>20060</code></td>
                  <td>Channel configuration not found</td>
                  <td>通道配置不存在，请联系运营人员</td>
                </tr>
                <tr>
                  <td><code>20060</code></td>
                  <td>Payment amount below minimum limit</td>
                  <td>支付金额低于最低限额</td>
                </tr>
                <tr>
                  <td><code>20060</code></td>
                  <td>Payment amount exceeds maximum limit</td>
                  <td>支付金额超过最高限额</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>代付下单错误码</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th style="width: 100px;">错误码</th>
                  <th style="width: 320px;">错误信息</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>20011</code></td>
                  <td>Merchant transfer channel not configured</td>
                  <td>商户未配置代付通道，请联系运营人员配置</td>
                </tr>
                <tr>
                  <td><code>20011</code></td>
                  <td>Transfer channel not enabled: xxx</td>
                  <td>代付通道未启用，请联系运营人员开启</td>
                </tr>
                <tr>
                  <td><code>20043</code></td>
                  <td>Transfer in progress, please do not repeat</td>
                  <td>转账处理中，请勿重复提交</td>
                </tr>
                <tr>
                  <td><code>20044</code></td>
                  <td>Order duplicate, please regenerate transfer</td>
                  <td>订单重复，请重新生成代付订单</td>
                </tr>
                <tr>
                  <td><code>20045</code></td>
                  <td>Insufficient balance</td>
                  <td>余额不足，请充值后重试</td>
                </tr>
                <tr>
                  <td><code>20060</code></td>
                  <td>Transfer amount below minimum limit</td>
                  <td>代付金额低于最低限额</td>
                </tr>
                <tr>
                  <td><code>20060</code></td>
                  <td>Transfer amount exceeds maximum limit</td>
                  <td>代付金额超过最高限额</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>订单查询错误码</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th style="width: 100px;">错误码</th>
                  <th style="width: 320px;">错误信息</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>20011</code></td>
                  <td>Merchant payment channel not configured</td>
                  <td>商户未配置支付通道</td>
                </tr>
                <tr>
                  <td><code>20011</code></td>
                  <td>Payment channel not enabled: xxx</td>
                  <td>支付通道未启用</td>
                </tr>
                <tr>
                  <td><code>20030</code></td>
                  <td>Query type not supported</td>
                  <td>查询类型不支持，请使用 1（代收）或 2（代付）</td>
                </tr>
                <tr>
                  <td><code>20041</code></td>
                  <td>Order not found</td>
                  <td>订单不存在，请检查订单号是否正确</td>
                </tr>
                <tr>
                  <td><code>20091</code></td>
                  <td>Order query failed: xxx</td>
                  <td>订单查询失败，具体原因见错误信息</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>商户余额查询错误码</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th style="width: 100px;">错误码</th>
                  <th style="width: 320px;">错误信息</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>20045</code></td>
                  <td>Insufficient balance</td>
                  <td>余额不足</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>通用错误码</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th style="width: 100px;">错误码</th>
                  <th style="width: 320px;">错误信息</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>20023</code></td>
                  <td>配置未启用</td>
                  <td>相关配置未启用，请联系运营人员</td>
                </tr>
                <tr>
                  <td><code>20025</code></td>
                  <td>配置不存在</td>
                  <td>相关配置不存在，请联系运营人员</td>
                </tr>
                <tr>
                  <td><code>20026</code></td>
                  <td>商户不存在</td>
                  <td>商户号不存在，请检查商户号是否正确</td>
                </tr>
                <tr>
                  <td><code>20052</code></td>
                  <td>验签失败</td>
                  <td>签名验证失败，请检查签名算法和密钥是否正确</td>
                </tr>
                <tr>
                  <td><code>30000</code></td>
                  <td>System error</td>
                  <td>系统未知错误，请联系技术支持</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>错误码总览</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th style="width: 100px;">错误码</th>
                  <th>错误类型</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td><code>20011</code></td>
                  <td>支付通道不存在</td>
                </tr>
                <tr>
                  <td><code>20023</code></td>
                  <td>配置未启用</td>
                </tr>
                <tr>
                  <td><code>20025</code></td>
                  <td>配置不存在</td>
                </tr>
                <tr>
                  <td><code>20026</code></td>
                  <td>商户不存在</td>
                </tr>
                <tr>
                  <td><code>20030</code></td>
                  <td>不支持该能力</td>
                </tr>
                <tr>
                  <td><code>20041</code></td>
                  <td>交易不存在</td>
                </tr>
                <tr>
                  <td><code>20043</code></td>
                  <td>交易处理中</td>
                </tr>
                <tr>
                  <td><code>20044</code></td>
                  <td>交易状态错误</td>
                </tr>
                <tr>
                  <td><code>20045</code></td>
                  <td>交易失败</td>
                </tr>
                <tr>
                  <td><code>20052</code></td>
                  <td>验签失败</td>
                </tr>
                <tr>
                  <td><code>20060</code></td>
                  <td>金额超过限额</td>
                </tr>
                <tr>
                  <td><code>20091</code></td>
                  <td>数据错误</td>
                </tr>
                <tr>
                  <td><code>30000</code></td>
                  <td>系统未知错误</td>
                </tr>
              </tbody>
            </table>
          </div>
        </div>

        <!-- 示例代码 -->
        <div id="download" class="section">
          <div class="content-header">
            <h2>示例代码</h2>
            <p>DCPAY 提供完整的示例代码，帮助您快速集成 API 接口。</p>
          </div>

          <div class="card">
            <h3>Java 示例</h3>
            <p>包含代收下单、代付下单、订单查询、余额查询、回调通知接收完整示例，纯 Java 标准库实现，无需第三方依赖。</p>
            <div style="margin-top: 20px;">
              <button class="download-btn" @click="downloadDemo">
                <span style="margin-right: 8px;">📥</span> 下载示例代码 (dcpayDemo.zip)
              </button>
            </div>
          </div>

          <div class="card">
            <h3>目录结构</h3>
            <div class="code-block">
              <pre><code>dcpayDemo/
└── src/
    ├── pay/                         # 代收下单
    │   ├── DcPayClient.java         # 客户端（签名、请求、解析）
    │   ├── PayRequest.java          # 请求参数封装
    │   ├── PayResponse.java         # 响应结果封装
    │   └── PayExample.java          # 调用示例
    ├── transfer/                    # 代付下单
    │   ├── TransferClient.java
    │   ├── TransferRequest.java
    │   ├── TransferResponse.java
    │   └── TransferExample.java
    ├── queryOrder/                  # 订单查询
    │   ├── QueryOrderClient.java
    │   ├── QueryOrderRequest.java
    │   ├── QueryOrderResponse.java
    │   └── QueryOrderExample.java
    ├── queryBalance/                # 商户余额查询
    │   ├── QueryBalanceClient.java
    │   ├── QueryBalanceRequest.java
    │   ├── QueryBalanceResponse.java
    │   └── QueryBalanceExample.java
    └── notifyUrlController/         # 回调通知接收（Spring Boot）
        └── NotifyController.java    # 接收 DCPAY 回调的 Controller 示例</code></pre>
            </div>
          </div>

          <div class="card">
            <h3>示例代码说明</h3>
            <table class="api-table">
              <thead>
                <tr>
                  <th>目录</th>
                  <th>文件</th>
                  <th>说明</th>
                </tr>
              </thead>
              <tbody>
                <tr>
                  <td rowspan="4"><code>pay/</code></td>
                  <td><code>DcPayClient.java</code></td>
                  <td>代收下单客户端（签名、请求、解析）</td>
                </tr>
                <tr>
                  <td><code>PayRequest.java</code></td>
                  <td>代收请求参数封装</td>
                </tr>
                <tr>
                  <td><code>PayResponse.java</code></td>
                  <td>代收响应结果封装</td>
                </tr>
                <tr>
                  <td><code>PayExample.java</code></td>
                  <td>代收调用示例</td>
                </tr>
                <tr>
                  <td rowspan="4"><code>transfer/</code></td>
                  <td><code>TransferClient.java</code></td>
                  <td>代付下单客户端（签名、请求、解析）</td>
                </tr>
                <tr>
                  <td><code>TransferRequest.java</code></td>
                  <td>代付请求参数封装</td>
                </tr>
                <tr>
                  <td><code>TransferResponse.java</code></td>
                  <td>代付响应结果封装</td>
                </tr>
                <tr>
                  <td><code>TransferExample.java</code></td>
                  <td>代付调用示例</td>
                </tr>
                <tr>
                  <td rowspan="4"><code>queryOrder/</code></td>
                  <td><code>QueryOrderClient.java</code></td>
                  <td>订单查询客户端（签名、请求、解析）</td>
                </tr>
                <tr>
                  <td><code>QueryOrderRequest.java</code></td>
                  <td>订单查询请求参数封装</td>
                </tr>
                <tr>
                  <td><code>QueryOrderResponse.java</code></td>
                  <td>订单查询响应结果封装</td>
                </tr>
                <tr>
                  <td><code>QueryOrderExample.java</code></td>
                  <td>订单查询调用示例</td>
                </tr>
                <tr>
                  <td rowspan="4"><code>queryBalance/</code></td>
                  <td><code>QueryBalanceClient.java</code></td>
                  <td>余额查询客户端（签名、请求、解析）</td>
                </tr>
                <tr>
                  <td><code>QueryBalanceRequest.java</code></td>
                  <td>余额查询请求参数封装</td>
                </tr>
                <tr>
                  <td><code>QueryBalanceResponse.java</code></td>
                  <td>余额查询响应结果封装</td>
                </tr>
                <tr>
                  <td><code>QueryBalanceExample.java</code></td>
                  <td>余额查询调用示例</td>
                </tr>
                <tr>
                  <td><code>notifyUrlController/</code></td>
                  <td><code>NotifyController.java</code></td>
                  <td>回调通知接收 Controller（Spring Boot），用于接收 DCPAY 订单状态变更通知</td>
                </tr>
              </tbody>
            </table>
          </div>

          <div class="card">
            <h3>使用说明</h3>
            <h4>环境要求</h4>
            <ul>
              <li>Java 11+（使用了 <code>java.net.http.HttpClient</code>）</li>
              <li>无需第三方依赖，开箱即用</li>
              <li><code>NotifyController.java</code> 需集成到 Spring Boot 项目中使用</li>
            </ul>

            <h4>配置参数</h4>
            <p>打开对应的 <code>*Example.java</code> 文件，修改以下参数：</p>
            <div class="code-block">
              <pre><code>String baseUrl = "https://api.dcpay.me";  // API 地址，固定不变
String mchNo = "DC1010";                  // 替换为您的商户号
String privateKey = "-----BEGIN PRIVATE KEY-----\n" +
    "您的商户私钥内容...\n" +
    "-----END PRIVATE KEY-----";</code></pre>
            </div>
            <p><code>NotifyController.java</code> 中需配置平台公钥（用于验证回调签名）：</p>
            <div class="code-block">
              <pre><code>private static final String PLATFORM_PUBLIC_KEY =
    "-----BEGIN PUBLIC KEY-----\n" +
    "您的平台公钥内容...\n" +
    "-----END PUBLIC KEY-----";</code></pre>
            </div>
            <p>商户号、私钥、平台公钥请登录 <a href="https://admin.dcpay.me/" target="_blank">商户端</a> → 设置 页面获取。</p>

            <h4>编译与运行</h4>
            <p>在 <code>dcpayDemo/</code> 根目录下执行：</p>
            <div class="code-block">
              <pre><code># 编译所有文件
javac -d out $(find src -name "*.java")

# 运行代收下单示例
java -cp out src.pay.PayExample

# 运行代付下单示例
java -cp out src.transfer.TransferExample

# 运行订单查询示例
java -cp out src.queryOrder.QueryOrderExample

# 运行余额查询示例
java -cp out src.queryBalance.QueryBalanceExample</code></pre>
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
                  <th style="width: 150px;">命令</th>
                  <th>说明</th>
                  <th style="width: 180px;">示例</th>
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
