# 中国移动组件服务 · 静音捕获版 (ChinaMobile Silent Capture)

Surge 模块。基于 ByteValley 作者的「中国移动组件服务」改造：**捕获参数照常进行，但可关闭每次打开 App 弹出的「✅ 数据捕获成功」系统通知**。开关与其它参数一样在模块参数里设置。

## 文件

| 文件 | 说明 |
|---|---|
| `ChinaMobile-Silent.sgmodule` | 配套模块（唯一需要导入 Surge 的文件；捕获规则指向本仓库 raw 的 10086-silent.js，其余 cron/面板/组件规则仍指向作者原版远程脚本、保持自动更新） |
| `10086-silent.js` | 改版脚本（基于作者 Prerelease-Alpha 10086.js，仅在弹通知调用前加 40 字节 capture_silent 判断，其余逐字节与原版一致） |

## 安装（Surge，在线单文件导入）

Surge → 模块 → 从 URL 导入，任选其一：

- GitHub raw：
  `https://raw.githubusercontent.com/vaeann/ChinaMobile-Silent/main/ChinaMobile-Silent.sgmodule`
- jsDelivr CDN（更快、带缓存，推荐）：
  `https://cdn.jsdelivr.net/gh/vaeann/ChinaMobile-Silent@main/ChinaMobile-Silent.sgmodule`

> 若之前已装原版「中国移动组件服务」模块，先停用原版再导入本模块，避免重复规则同时命中。

## 参数设置

导入后点模块 ⚙️ 参数设置，最下方新增一项 **「静默捕获」**：

- 填 `1`（或 `true`/`是`）→ 打开移动 App 自动捕获时**不再弹通知**，捕获正常写参数；
- 留 `0`/默认 → 与原版行为完全一致（照常弹通知）。

其余参数（手机号码、静默模式、Scriptable 服务模式等）含义与原版相同。

## 验证

- 开启「静默捕获=1」→ 杀掉移动 App 重开 → 应**没有**"数据捕获成功"弹窗；在 Surge 请求记录里仍能看到 autoLogin 命中捕获规则（捕获正常进行）。
- 想确认捕获成功：临时把「静默捕获」改回 0 重开一次 App，弹了「✅ 数据捕获成功」即说明链路正常。

## 注意

- 改版只影响"自动捕获成功"这一条系统通知；cron/面板的查询通知仍由「静默模式」控制。
- 改版脚本不随作者自动更新：作者发布新版后需对新版 10086.js 重打补丁（在 `_0x26673e(_0x2c2be0,_0x278d6e,_0x1518da);` 前加 `_0x5d2cc2(_0x2a970e['capture_silent'])||`）。
- 镜像：Gitee 同款文件在 `https://gitee.com/vaeann/vaeann/raw/master/cm-silent/ChinaMobile-Silent.sgmodule`（Gitee raw 对 Surge 下载偶发 404，故新增 GitHub 托管）。
