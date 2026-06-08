# Claude / AI 生态工具选用速览

> 把最近聊到的 Claude/AI 生态工具汇总成一页,方便挑选。
> 环境按 **Windows**(本机)给;命令型 `/plugin`、`/mcp` 在 Claude Code 里输入,与系统无关。
> ⚠️ 第三方工具接入前**看一眼源码/权限**,别盲接;标 beta 的别压关键流程。

---

## 一、先分清"类型"(很容易混)

| 类型 | 是什么 | 怎么用 |
|------|--------|--------|
| **CLAUDE.md 准则** | 一份指导文件,改善 AI 行为 | 合并进你的 `CLAUDE.md` |
| **Plugin(技能集)** | 一组 skills 打包 | `/plugin install` |
| **MCP Server** | 给 Claude 外接能力/工具 | `.mcp.json` 或 `claude mcp add` |
| **独立 App** | 自成一体的产品,不是 Claude 的插件 | 单独安装运行 |

> 详见 [hook/skill/subagent 关系与多级配置](Claude-Code实用技巧.md) 里对机制的区分。

---

## ★ 必装清单(先装这几个就够,3–6 个为宜)

社区共识:**GitHub + Context7 + Playwright 三个 MCP 覆盖约 80% 的编码工作流**。
不要全装——Claude Code 有 Tool Search 按需加载工具,装多了上下文不太炸,但**权限面和维护成本仍在**,最小必要最好。

**MCP — 高 ROI 优先装**
1. **GitHub MCP**(你已有)— 最高价值:让 Claude 参与 issue/PR 工作流。
2. **Context7** — 拉**真实、版本化的库文档**注入上下文,**专治"AI 编造 API"**;跨不熟的栈时尤其值。
3. **Playwright MCP** — 浏览器自动化/E2E/抓 JS 页面。`npx -y @playwright/mcp`,5 分钟搞定。

**按你的场景再加(数据/后端)**
- **数据库 MCP(Postgres / MySQL)** — 直接让 Claude 看表结构、跑**只读**查询,做数据活很省事(给只读账号)。
- **k6 MCP** — 压测时;但 Bash 直接跑 k6 也够。

**Skill / Plugin — 按需,不必都装**
- **Frontend Design**(官方)— 做 UI/前端时。
- **Anthropic 官方 skills**(`anthropics/skills`)— 要生成 docx/pptx/xlsx/pdf 时。
- **Superpowers** — 想要工程方法论(较重,挑有用的留)。

> ❌ 别装的:和 Claude Code 自带能力重复的(filesystem / fetch / 简单 web)——它已有文件/bash/web/搜索。

---

## 二、速览表

| 名称 | 类型 | 干什么 | 给谁 / 适不适合 |
|------|------|--------|----------------|
| **andrej-karpathy-skills** | CLAUDE.md 准则(社区) | 把 Karpathy 总结的 LLM 编码坑变成准则;"目标驱动+验证回路" | 想给 AI 立规矩的程序员;**非他本人仓库** |
| **Superpowers**(obra) | Plugin / 方法论 | TDD、系统化排查 bug、头脑风暴、子代理+代码评审 | **认真写代码的工程师**;重流程,快糙活会嫌烦 |
| **Frontend Design**(Anthropic 官方) | Plugin | 生成有设计感的前端 UI(多风格/配色/多栈) | 做前端/原型、又不擅长设计的人 |
| **k6 MCP**(QAInsights) | MCP | 自然语言跑 k6 压测 | 压测;但 Bash 直接跑 k6 也够,见 [k6压测实操](k6压测实操.md) |
| **JMeter MCP**(QAInsights) | MCP | 自然语言跑 JMeter | 已有 JMeter 资产者;AI 友好度不如 k6 |
| **Playwright MCP / Claude for Chrome** | MCP / 浏览器扩展 | 浏览器自动点击/填表/抓取 | E2E、表单自动化;真机操作用 Claude for Chrome(限量预览) |
| **OpenHuman**(tinyhumansai) | **独立 App** | 个人 AI 助理:连邮件/日历/GitHub/Notion,本地记忆 | 想要私有"个人事务助理";**早期 beta**;不替代写代码 |
| **画图** | 内置/外部 | mermaid(文本图,已在用)、draw.io(可导 Visio) | 技术图用 mermaid;UI 设计用 Figma;**没有 Visio skill** |

---

## 三、按需求怎么选

- **想让 AI 写代码更靠谱/有纪律** → Superpowers(留对你有用的,别被整套流程绑死)+ Karpathy 准则并进 CLAUDE.md。
- **做产品的前端/界面** → Frontend Design(出 UI);精细设计仍用 Figma。
- **压测** → 直接让 Claude 用 Bash 跑 **k6**(最省事);要自然语言编排再接 k6 MCP。
- **浏览器自动化** → 自动化测试/抓取用 **Playwright**;在自己真实浏览器代点用 **Claude for Chrome**。
- **打理个人数字生活(邮件/日历/任务)** → 试 **OpenHuman**,先连 2–3 个账号,别一上来全授权。

---

## 四、安装(Windows / Claude Code)

```text
# Plugin(在 Claude Code 里输入;marketplace 名以 /plugin marketplace 实际列表为准)
/plugin install superpowers@claude-plugins-official
/plugin install frontend-design@claude-plugins-official

# Karpathy 准则:克隆后把 CLAUDE.md 内容合并进你的项目 CLAUDE.md
git clone https://github.com/multica-ai/andrej-karpathy-skills

# MCP(k6 / JMeter):项目级写到仓库 .mcp.json,或全局
claude mcp add k6 -- <启动命令>
# 全局配置路径(Windows):%USERPROFILE%\.claude\
```

- **OpenHuman**:Windows 用官方安装包(直接下载安装器);它是独立桌面应用,不在 Claude 里装。
- MCP 的 k6/JMeter:云沙箱里发大压不现实(资源+出网受限),真压测去专用压测机。

---

## 五、通用注意

1. **看清类型**:别把"独立 App(OpenHuman)""CLAUDE.md 准则(Karpathy)"当成 Claude 的 skill 装。
2. **第三方信任**:MCP / 插件会拿到工具或账号权限,接前看源码、按最小授权。
3. **beta 别压关键流程**:OpenHuman 等早期项目先小范围试。
4. **别为花名买单**:名字花哨不代表适合你;留下真正省事的,其余删掉。

---

## 一句话总结

> **方法论(Superpowers/Karpathy)管"怎么写得稳",Frontend Design 管"界面好不好看",MCP(k6/Playwright)管"外接能力",OpenHuman 是另起炉灶的个人助理。** 按需求挑、按最小权限接、beta 别压重活——核心写代码仍以 Claude Code 为主。
