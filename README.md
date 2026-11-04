# llenger-vibecoding

面向 Codex 的轻量开发 Skills：从想法完成功能，从现象追查故障，均以验证和独立 Review 收尾。

| Skill | 流程 |
| --- | --- |
| [`$llenger-feature`](skills/llenger-feature/SKILL.md) | 需求讨论 → 任务书 → 开发 → 验证 → 独立 Review → 交付 |
| [`$llenger-debug`](skills/llenger-debug/SKILL.md) | 现象 → 根因证据 → 修复 → 回归验证 → 独立 Review → 交付 |

两个 Skill 仅显式调用。默认交付止于本地修改、验证与审查；创建分支、commit、push、PR 按当次用户指令执行。

## 前提

Codex 宿主需要提供子 Agent 调用能力和已配置的 `Reviewer` 角色，例如已有的 `~/.codex/agents/Reviewer.toml` 或项目级同名配置。项目复用该角色，不提供角色 TOML，不依赖 `$team-mode`，不改变现有模型和角色设置。

缺少 Reviewer 时仍可执行可完成的开发与验证，但会明确报告“独立 Review 未完成”。

## 本地安装

在仓库根目录执行，将两个 Skill 链接到用户级目录；同名目标必须尚不存在：

```sh
mkdir -p "$HOME/.agents/skills"
ln -s "$PWD/skills/llenger-feature" "$HOME/.agents/skills/llenger-feature"
ln -s "$PWD/skills/llenger-debug" "$HOME/.agents/skills/llenger-debug"
```

链接保留仓库作为唯一源码，移动仓库后需要更新链接。`ln -s` 不覆盖同名目标。若 Skill 未出现，重启 Codex 后重试。

在目标项目的对话中调用：

```text
$llenger-feature 给任务列表增加按状态筛选，先结合现有代码收敛需求。

$llenger-debug 搜索结果翻到第二页后切换筛选条件，页面显示为空，请追查并修复。
```

安装目录、软链接支持与显式调用策略依据 [OpenAI 官方 Skill 文档](https://learn.chatgpt.com/docs/build-skills)。

## 任务书与验收

已确认范围见 [任务书](docs/task-spec.md)。两个 Skill 为纯指令实现，不附带安装脚本或运行时依赖。

结构校验、显式调用配置、本机发现检查和隔离样例验证已通过，独立审查无实质发现；详见 [验证记录](docs/validation.md)。真实 Feature 与真实故障的首次使用验收尚待完成，不以样例验证代替。
