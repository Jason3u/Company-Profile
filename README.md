# Company Profile

一份面向个人使用的 Clash 分流配置，适用于 **Mihomo / Clash Meta** 内核，可导入 Clash Verge Rev、Mihomo Party、FlClash 等兼容客户端。

配置文件：[Company-Profile.yaml](./Company-Profile.yaml)

## 分流策略

规则按从上到下的顺序匹配，优先级如下：

| 流量类型 | 默认策略 | 可手动切换 |
| --- | --- | --- |
| Adobe 相关域名 | 拦截（`REJECT`） | 否 |
| 内网及本地流量 | 直连 | 否 |
| ChatGPT、Claude 等 AI 服务 | 美国节点 | 自动选择 |
| RunningHub | 香港节点 | 自动选择 |
| TapNow | 香港节点 | 日本节点、自动选择 |
| YouTube、Netflix、Pinterest 等流媒体 | 日本节点 | 香港节点、自动选择、直连 |
| 中国大陆网站与 IP | 直连 | 否 |
| 其他未命中流量 | 节点选择 | 任意可用节点或策略组 |

> 美国节点默认仅用于 AI 服务，以减少因频繁切换地区导致的账号风控风险。

## 使用方法

### 1. 准备订阅

打开 `Company-Profile.yaml`，确认 `proxy-providers.Provider.url` 是你自己的有效订阅地址。建议不要把包含 token 的真实订阅地址提交到公开仓库。

```yaml
proxy-providers:
  Provider:
    type: http
    url: "你的订阅地址"
```

### 2. 导入配置

#### 本地导入

1. 下载 `Company-Profile.yaml`。
2. 打开 Clash 客户端的订阅或配置页面。
3. 选择“导入本地文件”，然后选中下载的 YAML 文件。
4. 启用该配置并更新一次订阅。

#### 通过 URL 导入

公开仓库可使用以下地址：

- GitHub Raw：`https://raw.githubusercontent.com/Jason3u/Company-Profile/main/Company-Profile.yaml`
- jsDelivr：`https://cdn.jsdelivr.net/gh/Jason3u/Company-Profile@main/Company-Profile.yaml`

使用 URL 导入后，仓库中的配置更新可以由客户端重新拉取。GitHub Raw 或 CDN 可能存在缓存延迟。

### 3. 选择节点

导入成功后，在客户端的“代理”页面进行选择：

- `🚀 节点选择`：默认出口，可选择自动测速或指定节点。
- `🌍 流媒体专用`：默认使用日本节点，也可切换到香港。
- `🤖 AI专用`：默认使用美国节点。
- `🎨 RunningHub`：默认使用香港节点。
- `🎨 TapNow`：默认使用香港节点，也可切换到日本。

节点选择会由客户端保存，重启后通常无需重新设置。

## 自定义配置

### 添加域名规则

在 `rules` 中添加规则，并放到兜底规则 `MATCH` 之前：

```yaml
- DOMAIN-SUFFIX,example.com,🚀 节点选择
- DOMAIN-SUFFIX,example.cn,🎯 国内直连
- DOMAIN-SUFFIX,example.net,🛑 拦截
```

规则从上到下匹配，越具体、优先级越高的规则应放得越靠前。

### 调整地区节点

日本、香港和美国节点池通过 `proxy-groups` 中的 `filter` 正则筛选节点名称。如果订阅服务使用了不同的地区命名，请相应修改正则表达式。

### 恢复 Adobe 联网

当前配置会拦截 Adobe 主域名、关联产品及部分统计域名。需要正常使用 Adobe 在线服务时，请删除 `rules` 中 Adobe 拦截段；仅切换代理节点不会解除拦截。

## 常见问题

### 订阅更新失败或没有节点

- 检查 `proxy-providers.Provider.url` 是否有效或已经过期。
- 在客户端中手动更新订阅。
- 检查当前网络能否访问订阅服务。
- 确认客户端使用 Mihomo / Clash Meta 内核。

### 某个地区节点池为空

订阅中的节点名称可能无法匹配现有 `filter`。根据实际节点名称调整对应地区节点池的筛选正则。

### 网站分流不符合预期

- 检查配置是否处于 `rule` 模式。
- 查看客户端连接日志，确认实际命中的规则。
- 将自定义规则放在 `GEOSITE,cn`、`GEOIP,CN` 和最终 `MATCH` 规则之前。

### Adobe 官网或应用无法联网

这是 Adobe 拦截规则的预期行为。需要恢复访问时，请参照上方“恢复 Adobe 联网”。

## 安全提示

- 不要在公开仓库中提交包含 token、用户名或密码的订阅地址。
- 如果订阅地址曾公开，请立即到服务商后台重置订阅 token，并更新本地配置。
- 建议将含真实订阅地址的配置保存在私有仓库，或只保留在本地。
- 分享配置前，请用占位地址替换 `proxy-providers.Provider.url`。

## 配置检查

修改 YAML 后，建议先使用客户端的配置校验功能确认语法无误，再启用新配置。若客户端提示不支持 `GEOSITE`、`GEOIP` 或 `proxy-providers`，请升级到较新的 Mihomo / Clash Meta 内核。
