# llenger-vibecoding

面向 Codex 与 ZCode 的轻量开发 Skills：理解项目、完成功能、追查故障；开发与修复以验证和独立 Review 收尾。

| Skill | 流程 |
| --- | --- |
| [`$llenger-how`](skills/llenger-how/SKILL.md) | 只读理解 → 运行链路与部署关系 → 迭代入口 |
| [`$llenger-feature`](skills/llenger-feature/SKILL.md) | 需求讨论 → 任务书 → 开发 → 验证 → 独立 Review → 交付 |
| [`$llenger-debug`](skills/llenger-debug/SKILL.md) | 现象 → 根因证据 → 修复 → 回归验证 → 独立 Review → 交付 |

三个 Skill 仅显式调用。How 默认只读、在对话交付；Feature、Debug 默认交付止于本地修改、验证与审查。创建分支、commit、push、PR 按当次用户指令执行。

## 前提

Feature、Debug 需要 Codex 宿主提供子 Agent 调用能力和已配置的 `Reviewer` 角色，例如已有的 `~/.codex/agents/Reviewer.toml` 或项目级同名配置。项目复用该角色，不提供角色 TOML，不依赖 `$team-mode`，不改变现有模型和角色设置。

缺少 Reviewer 时仍可执行可完成的开发与验证，但会明确报告“独立 Review 未完成”。How 由主 Agent 完成，不依赖 Reviewer，也不是开发前的必经步骤。

## 本地安装

在仓库根目录执行，将三个 Skill 链接到用户级目录；同名目标必须尚不存在：

```sh
mkdir -p "$HOME/.agents/skills"
ln -s "$PWD/skills/llenger-how" "$HOME/.agents/skills/llenger-how"
ln -s "$PWD/skills/llenger-feature" "$HOME/.agents/skills/llenger-feature"
ln -s "$PWD/skills/llenger-debug" "$HOME/.agents/skills/llenger-debug"
```

链接保留仓库作为唯一源码，移动仓库后需要更新链接。`ln -s` 不覆盖同名目标。若 Skill 未出现，重启 Codex 后重试。

在目标项目的对话中调用：

```text
$llenger-how 帮我理解这个开源项目，重点看部署方式、模型请求链路，以及接入自定义 provider 应该改哪里。

$llenger-feature 给任务列表增加按状态筛选，先结合现有代码收敛需求。

$llenger-debug 搜索结果翻到第二页后切换筛选条件，页面显示为空，请追查并修复。
```

安装目录、软链接支持与显式调用策略依据 [OpenAI 官方 Skill 文档](https://learn.chatgpt.com/docs/build-skills)。

## 在 ZCode 中使用

三个 Skill 是宿主中立的纯指令实现。ZCode 同样从 `~/.agents/skills` 发现它们，并按 `/llenger-how`、`/llenger-feature`、`/llenger-debug` 显式调用；各 Skill 包内的 `agents/openai.yaml` 仅作用于 Codex 的隐式触发开关，ZCode 忽略该文件，无需任何改动。

Feature、Debug 的独立 Review 在 ZCode 侧复用用户级角色 `~/.zcode/agents/Reviewer.md`，职责与 Codex 的 `~/.codex/agents/Reviewer.toml` 对齐：独立审查、不预设根因、报告问题位置/证据/严重程度或明确无实质发现、不改被审代码、不派生子代理。Skill 中的 `fork_turns="none"` 对应 ZCode 子代理每次调用的全新上下文。仓库仍不随附任何角色定义；缺少该角色时按降级策略执行，明确报告“独立 Review 未完成”。

## 任务书与验收

已确认范围见 [任务书](docs/task-spec.md)。三个 Skill 为纯指令实现，不附带安装脚本或运行时依赖。

原有 Feature、Debug 已完成结构校验、本机发现检查、隔离样例验证与独立审查；用户于 2026-11-09 反馈实战使用良好。本次 How 及规则增强的验证范围与局限见 [验证记录](docs/validation.md)。

## 设计参考

选择性借鉴 [pstack](https://github.com/cursor/plugins/tree/main/pstack) 的 how、architect、真实路径验证、根因判断、共同前提检验与行为测试理念，按本项目的主 Agent 工作方式重新编写，不引入其模型配置或多代理编排。How 用于理解系统；已有 handoff 继续承接工作进度。
