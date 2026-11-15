# doubao-tiaforge —— 豆包（豆包工作）接西门子博途

让豆包直接读写**西门子博途（TIA Portal）**工程：查文档、生成梯形图、导入前先看图、云端跑仿真、在真实工程里编译下载读回。

装法只有一步：**上传插件包**。包里带 **6 个技能 + 5 个连接**。

```
查文档 → 生成梯形图 → 渲染核对 → 云端仿真 → 真实工程编译/下载/读回
```

## 下载

[tiaforge-doubao-plugin-1.0.5.zip](https://github.com/tia-forge/doubao-tiaforge/releases/download/v1.0.5/tiaforge-doubao-plugin-1.0.5.zip)（41 KB）

## 安装

1. 豆包客户端左侧「**插件 · 技能 · 伙伴**」→「插件」→ 右上角 `+ 添加` → **上传插件包** → 选 zip，不用解压；
2. 装完先去「**连接器**」把 `tialink` 的地址改成你的网关（**先改参数、后开开关**，反了会一直"运行失败"）：

   | 网关在哪 | 服务器 URL |
   |---|---|
   | 这台电脑 | `http://127.0.0.1:8080/mcp` |
   | 另一台 Windows | `http://<那台的局域网IP>:8080/mcp` |

   点 `tialink` 右侧 `…` →「编辑」→ 改「服务器 URL」→ 保存 → 再打开开关（或点「重新启动」）。
3. **完全退出豆包再打开**（MCP 连接是启动时建立的），然后**新开一个任务**。

## 包里有什么

| 类型 | 内容 | 作用 |
|---|---|---|
| 技能 ×6 | 博途工程直连 / 文档检索 / LadDoc 转 SimaticML / 梯形图渲染 / 云仿真 / 编程教练 | 告诉它什么时候用、按什么顺序做 |
| 连接 ×5 | TiaLink + 文档检索 + 云仿真 + LadDoc + 梯形图渲染 | 给它真正能调的工具 |

技能和连接缺一不可：**只有技能，它知道怎么做但没有手；只有连接，它有手但不知道你的流程。**

云端四个连接各要一把 API Key，每把 100 次免费：<https://tia.hubwiz.com/tia-link>

## 怎么确认"真接上了"

配好之后**不用再报地址**——直接说人话就会触发：

> 现在打开的是哪个博途工程？有几台设备？

- 报出**你屏幕上的工程名和设备型号** → 通了；
- 给你一段代码、或"建议你这样操作" → **没通**，它在凭记忆答。

> 只有在"刚配好、怀疑没生效"时才需要把地址说出来排查：
> `连接 http://127.0.0.1:8080/mcp，告诉我现在打开的是哪个工程、有几台设备`。
> 如果每次都得报地址才动，说明连接器没真正生效——回去看「先改参数、后开开关」那步。

再问「当前工程里有几个程序块」，它会直接给块清单（实测 24 秒读出 8 个块：1 OB + 1 FB + 2 FC + 4 DB）。

## 已知限制

- **TiaLink 必须与博途同一台 Windows**，但那台机器不必是你的电脑（填局域网 IP 即可）。
- **未激活的 TiaLink 只读**：能读工程、不能写，也不会擅自动你的工程。
- **回环地址不能写进插件包**：包里默认是局域网占位地址；本机网关请通过「编辑」改成 `127.0.0.1`。
- **自定义连接器只在本地电脑生效**（弹窗原话），云端任务里找不到工具。
- 换网关 IP 只需「编辑」，不用重装包；换电脑要重新装。
- 没填 Key 的云端连接会显示"运行失败"——那是 **401 未授权**，不是网络问题。

## 仓库结构

```
plugin/                     ← 插件包源码（打 zip 直接上传）
  plugin.json               ← 根清单：name / version / 5 个服务的展示信息
  mcp.json                  ← 5 个连接（tialink 为占位局域网地址）
  com.doubao/auth/*.json    ← 4 份云端 API Key 的鉴权声明
  skills/*/SKILL.md ×6      ← 6 个技能
```

技能正文的单一真源在 [tia-forge/tiaforge-plugins](https://github.com/tia-forge/tiaforge-plugins)（SkillHub → 各客户端同步）；本仓只负责豆包的打包与分发。

## 相关

- 其他客户端：DSH / WorkBuddy / Claude Code / pi / Cursor → [tia-forge/tiaforge-plugins](https://github.com/tia-forge/tiaforge-plugins)
- 网关本体（TiaLink）：[tia-forge/tialink](https://github.com/tia-forge/tialink) ｜ 产品页 <https://tia.hubwiz.com/tia-link>
- 图文说明：《怎么把豆包和博途连接起来》<https://www.hubwiz.com/blog/doubao-tia-portal-three-steps/>
