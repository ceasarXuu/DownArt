# AGENTS.md

<!-- 本文件是本仓库的项目级指令与约束，面向在本仓库工作的 AI 编码助手（agent）。
     内容由仓库作者维护；标注 TODO 的条目尚未定稿，agent 不得自行假定其结论。 -->

## 1. 项目概述

- 名称：DownArt
- 定位：**个人使用**的移动端 App，**开源**、**不商业化**
- 形态：使用 Expo 构建的跨端 App（iOS + Android）
- 核心功能：集成若干开源工具，对主流内容平台的公开内容进行抓取、下载与内容管理

TODO: 一句话产品定位、目标使用场景、与同类工具的差异

## 2. v0.0.1 范围

- 支持平台：Twitter（X）、Instagram
- 支持内容类型：图片、视频

TODO: 明确「不包含」的范围（例如：音频、直播、Story、私密账号、批量订阅/定时抓取等）
TODO: 页面与交互清单（解析入口 / 下载任务队列 / 本地内容库 / 设置 等）
TODO: 单条与批量下载的边界，以及失败重试策略

## 3. 技术栈与约定

- 框架：Expo（React Native）
- 调试方式：优先使用 **Expo Go** 快速调试，暂不依赖本地 iOS / Android 原生开发环境
- 本机开发环境：已就绪

TODO: 语言与严格模式（TypeScript？）、包管理器、Node 版本要求
TODO: 目录结构约定（app / src / components / platforms 适配层等）
TODO: 代码风格、lint / format 规则、提交信息规范
TODO: 状态管理、数据持久化、网络请求层方案
TODO: 抓取实现路线（纯 HTTP 接口解析 / WebView 注入 / 复用开源库），以及各平台适配的抽象边界

## 4. 应用标识

| 平台 | 包名 / Bundle ID |
| --- | --- |
| iOS | `nbspace.downart.ios` |
| Android | `nbspace.downart.android` |

## 5. 发布与分发

- iOS：**暂不发布**
- Android：通过 **GitHub Release** 分发
- 构建方式：**GitHub Actions（GitHub CI）**，不使用 EAS Build

实现路线：GitHub 托管的 `ubuntu-latest` runner 已预装 Android SDK，可在 CI 内执行
`npx expo prebuild --platform android` → `./gradlew assembleRelease` 产出 APK，
再上传至 GitHub Release。因此无需在本机安装 Android 原生开发环境，
与「不依赖本地原生环境」的约束一致。

TODO: 签名密钥（keystore）的生成与保管方式，以及如何通过 GitHub Actions Secrets 注入（禁止将密钥提交入库）
TODO: 版本号规则（versionCode / versionName 递增方式）、Release 产物命名、changelog 维护方式
TODO: workflow 文件待 Expo 工程骨架建立后再落地（当前仓库尚无 package.json，此时写 workflow 只会产生空构建）

## 6. 许可证与合规

- 许可证：**Creative Commons Attribution-NonCommercial 4.0 International（CC BY-NC 4.0）**
  - 全文见仓库根目录 `LICENSE`；规范链接 https://creativecommons.org/licenses/by-nc/4.0/
  - 非商业限制依据：Legal Code §2(a)(1)，仅授予 "for NonCommercial purposes only" 的使用权
  - 免责依据：Legal Code §5（Disclaimer of Warranties and Limitation of Liability）
- 声明**仅用于学习研究**，作者不承担版权相关责任
- 本项目属于 **source-available（源码可见 + 禁止商用）**，**不是 OSI 定义的开源许可证**。
  对外描述时不要自称「OSI 开源」或「开源协议」。
- 已知取舍（作者已确认接受，不必重复讨论）：
  - CC 官方明确建议不要将 CC 许可证用于软件："CC licenses are appropriate for all types of
    content you want to share publicly, **except software and hardware**"
    （https://wiki.creativecommons.org/wiki/Considerations_for_licensors_and_licensees）
  - CC BY-NC 4.0 Legal Code §2(b)(2)："Patent and trademark rights are not licensed under
    this Public License"，即不授予专利许可（对比：Apache-2.0 含专利授权条款）
  - 免责声明是必要声明，但不构成对第三方版权主张的免疫；作者仍需对自身行为负责

TODO: 确认 `LICENSE` 中的著作权人署名（当前写的是 GitHub 账号 `ceasarXuu`、年份 2026；需真实姓名请替换）
TODO: 所集成第三方开源工具的许可证清单与署名（NOTICE / THIRD-PARTY）；
      注意 CC BY-NC 4.0 与 GPL / AGPL 类依赖不兼容，挑选依赖时需核对
TODO: 免责声明文本及其展示位置（App 首次启动）；`LICENSE` 与 README 已含基础版本

## 7. Agent 工作约定

### 代码管理

* 除非创建PR用于专题提交，否则未经用户允许不得创建新分支；确有必要时先申请确认。
* 未经允许禁止修改 worktree 的分支绑定
* 使用原子提交：完成一个可独立理解且已基本验证的改动后积极 commit 并 push 到远端，不得遗留当前任务产生的未提交修改。
* 发现任务开始前已有或来源不明的修改时，必须保留并避免覆盖、reset 或擅自归属；不能以此为理由忽略自己产生的修改。
* 未经用户明确要求，不得执行不可恢复或可能破坏现有数据的删除操作。正常的 Git 可恢复删除不受此限制；存在明显数据损失风险时必须先取得批准。
* 原则上单个手写源代码文件不得超过 500 行；确有必要时须说明原因并取得用户批准。
* 未经用户明确批准，不得在当前批准的实现阶段内新增超过 500 行手写生产代码。测试、生成代码、测试用例、快照、锁文件和机械迁移不计入。预计超过限制或实际复杂度明显超出批准计划时，立即停止扩张，重新检查是否存在更简单直接的方案，并取得用户批准后继续。
* docs/ 默认使用中文，但已有项目规范或用户明确要求其他语言时遵循项目规范。

### 工作模式

* 优先遵循仓库已有的开发、版本、架构、测试、文档和交付规范。
* 保持按“版本“+“主题“（版本下的feature和 fix） +“issue"（一个主题或者主题内的更小颗粒度）三层粒度工作，issue 保持在对应的 github repo 中更新同步。
* 追求“最小充分设计”：首先保证根因正确、行为正确和架构一致，在满足这些条件的方案中优先选择复杂度和改动范围最小的实现。不得仅为了架构纯洁、未来可能需求或设计完整性主动扩大当前范围。
* 只有存在明确证据表明局部修复会继续产生错误抽象、重复逻辑、状态不一致或明显维护风险时，才考虑结构性重构；若重构明显扩大用户批准范围，应先取得批准。
* 涉及外部 API、框架行为、不熟悉技术、重大技术路线或不确定事实时，优先查官方文档、源码/issue 和成熟社区实践；已有项目内部模式能够正确解决的问题优先保持项目一致性，避免为了套用外部模式重新设计。
* 涉及尚未由用户明确确认的核心产品行为、用户体验、数据模型、业务规则、权限边界，或会产生重大长期影响的技术路线时，不得自行把假设升级为用户决策，应与用户确认。建议必须说明依据；涉及外部技术事实时优先提供一手资料。
* 当出现重复失败、连续审查修复仍产生新问题、范围持续扩大、开始修改与目标无直接关系的模块，或方案依赖越来越多特殊 case 时，停止继续扩张并重新检查目标、根因和证据；必要时请求用户介入。
* 代码变更采用风险相匹配的验证：优先运行最小相关测试，高风险变更再扩大回归范围。缺乏测试基础设施时，不得仅为了当前小改动自动建设大型测试体系；若测试建设会明显扩大范围，应取得用户批准。
* 修改功能或修复故障时检查现有可观测性是否足以定位关键失败路径；只有存在明确诊断价值时才补充日志或指标，避免冗余日志、敏感数据和无价值噪声。
* 记录非显而易见、项目特有、未来大概率重复使用且已经实践验证的操作经验，记录到项目根目录 /runbooks 目录下；优先更新已有 runbook，不为一次性问题制造文档。
* 在任务可清晰拆分、子任务低耦合且并行收益明显时使用 Subagent；简单或高度耦合任务优先直接完成，避免无必要的多 Agent 编排和循环审查。
* 完成一个用户级任务或明确实现阶段后，根据变更风险判断是否建议执行对抗性审查；小型、机械性、低风险变更无需主动建议。对抗性审查必须经用户批准后执行。

### Skills

以下情景优先选择的 skills
* 新增或实质性变更需求： `clear-prd`。
* Debug： `bug-killer`。
* 需要正式进度汇报： `plan-report`。
* 需要制定开发计划： `se-good-plan`。
* 需要执行审查： `subagent-vs-review`

### 其他工具提醒
- 未经批准禁止使用iphone镜像功能
- 未经批准禁止新建 iphone/ipad 模拟器