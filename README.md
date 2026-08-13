# Company Profile

个人 Clash 分流配置（Clash Meta / Mihomo 内核，兼容 Clash Verge Rev、Mihomo Party、FlClash 等客户端）。

## 分流策略

| 流量类型 | 去向 |
|---|---|
| 国内网站 / 应用 | 直连 |
| 国外软件 / 网站 | 代理（默认走「🚀 节点选择」，可在客户端里手动切换） |
| YouTube / Pinterest / Netflix 等流媒体 | 日本 / 香港节点（默认日本，可手动切换） |
| ChatGPT / Claude 等 AI 服务 | 美国节点 |
| Adobe 全家桶 | 全部断网（REJECT，防止弹购买提示） |
| 广告 / 追踪域名 | 拦截 |

## 快速开始

### 1. 填入你的订阅链接

打开 `Company-Profile.yaml`，找到 `proxy-providers` 段：

```yaml
proxy-providers:
  Provider:
    type: http
    url: "https://请在这里填入你的订阅链接"   # ← 把这里替换成你的机场订阅链接
```

订阅链接一般在机场网站「复制订阅链接」按钮处获取。

### 2. 导入到 Clash

**方式 A：本地文件导入**

下载 `Company-Profile.yaml` → 打开 Clash Verge Rev →「订阅」→「导入」→ 选择本地文件。

**方式 B：URL 导入（推荐，以后更新规则直接改仓库）**

在客户端「导入」页面粘贴下面的地址：

- GitHub 直连：`https://raw.githubusercontent.com/Jason3u/Company-Profile/main/Company-Profile.yaml`
- jsDelivr 加速（国内更稳）：`https://cdn.jsdelivr.net/gh/Jason3u/Company-Profile@main/Company-Profile.yaml`

### 3. 选择节点

- 打开「代理」面板，把 **🚀 节点选择** 选中为「♻️ 自动选择」或具体某个节点。
- 流媒体默认走日本：可在 **🌍 流媒体专用** 里切到「🇭🇰 香港节点」。
- AI 默认走美国：可在 **🤖 AI专用** 里切换。

## 自定义规则

- **新增断网 / 直连 / 分流的域名**：在 `rules` 段对应位置加一行，格式 `DOMAIN-SUFFIX,xxx.com,组名`。
- **Adobe 断网列表**：位于 `rules` 第一段，按需增删；若日后购买正版，请删除整个第一段。
- **节点地区筛选**：在 `proxy-groups` 里各节点池的 `filter` 正则处调整（例如日本节点池只保留含「日本 / JP / Tokyo」的节点）。

## 常见问题

- **订阅拉取失败 / 没有节点**：检查订阅链接是否过期，或重新复制；可在客户端里手动「更新订阅」。
- **规则源拉取失败**：删除 `rule-providers` 中的 `reject` 段，以及 `rules` 中 `RULE-SET,reject` 那一行（仅影响广告拦截，不影响核心分流）。
- **Adobe 相关应用打不开 / 官网无法访问**：这是「🛑 拦截」规则的预期效果，说明断网生效了。
- **首次运行需要联网下载 geoip.dat / geosite.dat**：客户端会自动处理，稍等片刻即可。

## ⚠️ 安全提醒

本仓库是**公开**的：**不要把真实订阅链接（含 token）提交到仓库里**，否则任何人都能使用你的订阅流量。建议把真实链接填在本地使用的文件里；如担心泄露，可将仓库改为私有。
