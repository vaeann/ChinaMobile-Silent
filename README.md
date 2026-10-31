# 中国移动组件服务 · 改造版（静音捕获 / 通知开关）

Surge 模块。基于 ByteValley 作者的「中国移动组件服务」改造，解决两条**写死的系统通知**：

1. 打开中国移动 App 自动捕获参数时必弹「✅ 数据捕获成功」；
2. 桌面小组件每次刷新查询成功时必弹「数据刷新成功」（连刷会连弹）。

改版把这些通知做成模块参数开关：**功能照常执行，只是可以选择不弹通知**。

## 两个版本

| 版本 | 脚本 | 模块 | 参数 |
|---|---|---|---|
| **通知开关版（推荐）** | `10086-notify.js` | `ChinaMobile-Notify.sgmodule` | 静默捕获 + **静默组件刷新** |
| 静音捕获版 | `10086-silent.js` | `ChinaMobile-Silent.sgmodule` | 仅静默捕获 |

> 通知开关版是静音捕获版的**超集**：两个静音开关都有，直接用通知开关版即可。

## 文件

| 文件 | 说明 |
|---|---|
| `ChinaMobile-Notify.sgmodule` | 通知开关版模块（捕获/组件两条规则指向本仓库 raw 的 `10086-notify.js`，其余 cron/面板规则仍指向作者原版、保持自动更新） |
| `10086-notify.js` | 通知开关版脚本（在原作者 Prerelease-Alpha 10086.js 基础上加 `capture_silent` + `widget_silent` 两个判断，其余逻辑不动） |
| `ChinaMobile-Silent.sgmodule` | 静音捕获版模块（旧版，保留兼容） |
| `10086-silent.js` | 静音捕获版脚本（旧版，仅 `capture_silent`） |

## 安装（Surge，在线单文件导入）

Surge → 模块 → 从 URL 导入，任选其一：

- **通知开关版 · GitHub raw（推荐）**
  `https://raw.githubusercontent.com/vaeann/ChinaMobile-Silent/main/ChinaMobile-Notify.sgmodule`
- **通知开关版 · jsDelivr CDN（更快、带缓存）**
  `https://cdn.jsdelivr.net/gh/vaeann/ChinaMobile-Silent@main/ChinaMobile-Notify.sgmodule`
- 静音捕获版（旧）
  `https://raw.githubusercontent.com/vaeann/ChinaMobile-Silent/main/ChinaMobile-Silent.sgmodule`

> 若之前已装原版「中国移动组件服务」模块，先停用原版再导入，避免重复规则同时命中。

## 参数设置

导入后点模块 ⚙️ 参数设置，最下方两项：

| 参数 | 填 1 / true / 是 | 留 0 / 默认 |
|---|---|---|
| **静默捕获** | 打开 App 自动捕获参数时**不再弹**「数据捕获成功」，捕获照常 | 照常弹通知 |
| **静默组件刷新** | 桌面小组件刷新查询时**不再弹**「数据刷新成功」 | 照常弹通知 |

其余参数（手机号码、静默模式、Scriptable 服务模式、CRONEXP、BarkKey 等）含义与原版完全相同。

## 验证

- **静默捕获**：开「静默捕获=1」→ 杀掉移动 App 重开 → 应没有「✅ 数据捕获成功」弹窗；Surge 请求记录里仍能看到 autoLogin 命中捕获规则即说明捕获正常。
- **静默组件刷新**：开「静默组件刷新=1」→ 等组件刷新或手动触发 → 面板/桌面不再弹「数据刷新成功」；组件数据仍在更新。
- 想确认链路本身正常：把对应开关改回 0 再触发一次，弹了通知即说明原逻辑没坏。

## 注意

- 两个开关互不影响，也与原「静默模式」（只管 cron/面板查询通知）互不影响。
- 改版脚本不随作者自动更新：作者发布新版 10086.js 后需对新版重打补丁，或回来找我重做。
- 本改版仅加静默判断，不改变查询、解密、上下文写入等任何业务逻辑。
