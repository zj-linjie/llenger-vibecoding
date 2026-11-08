# llenger-vibecoding 任务书

2026-11-04 创建；2026-11-09 按用户确认新增 How，并增强 Feature、Debug。

## 目标与范围

提供三个自包含的轻量 Skill，支持 Codex 与 ZCode：

- `$llenger-how`：只读理解项目或子系统 → 运行链路与部署关系 → 后续迭代入口；与已有 handoff 分工。
- `$llenger-feature`：想法 → 需求讨论 → 任务书 → 开发 → 验证 → 独立 Review → 交付。
- `$llenger-debug`：截图、报错或现象 → 调查与根因证据 → 修复 → 回归验证 → 独立 Review → 交付。

需求明确时直接推进；模糊时先结合项目上下文调查，再讨论影响实现的关键问题。Feature 任务书包含目标、范围、实现思路、验收条件和待定事项；Debug 诊断记录包含现象、复现方式、根因证据、修复方案和验证方法。

## 已确认的共同约束

- 仅显式调用，通过各自 `agents/openai.yaml` 中的 `policy.allow_implicit_invocation: false` 关闭隐式触发。
- How 由主 Agent 只读调查，默认在对话交付，不依赖 Reviewer，不作为其他 Skill 的前置步骤。
- Feature、Debug 由主 Agent 负责实现、验证、修正审查发现的问题与最终验收；现有 Reviewer 以新鲜上下文独立审查稳定后的变更。
- 不增加其他强制子 Agent，不启动完整 Team Mode，不新增 Python 脚本、调度器或角色 TOML；保留现有 Team Mode。
- Reviewer 只审查相关变更，涵盖相关未跟踪文件；存在阻塞问题时由主 Agent 修正，并按需复审。
- 无法复现、未执行测试、Reviewer 不可用以及剩余风险均如实报告，不编造成功。
- 默认交付止于本地修改、必要验证和独立审查。分支、commit、push、PR 和其他外部写入按当次指令执行。
- 项目不捆绑模型、角色配置或运行时依赖；安装时复用目标环境已有的 Reviewer。

## 验收

1. 三个目录均符合 Skill 标准，名称与目录一致；显式调用策略正确。
2. 使用一次真实小型 Feature：从需求收敛到实现、验证、独立 Review 和交付，且不擅自执行 Git 交付操作。
3. 使用一次真实截图或故障描述：能自主定位，给出可核查的根因证据，修复并验证，完成独立 Review。
4. Reviewer 不可用或证据不足时明确报告受阻环节，不冒充完成。
5. How 围绕具体问题提供可核查的运行链路和修改入口，区分源码事实与部署观察，保持只读边界。
6. Feature 在跨模块修改前确认职责及扩展点；Debug 判断触发状态合法性，并在同一前提下两次修复失败后直接验证该前提。两者按风险验证真实路径及实际构建，以行为契约检验测试质量。

结构校验、隔离样例验证与真实项目验收分别记录；前两者不能替代真实使用验收。

## 来源

- [ZJ-24 · Feature Skill](https://linear.app/zjlinjie/issue/ZJ-24)
- [ZJ-25 · Debug Skill](https://linear.app/zjlinjie/issue/ZJ-25)
- [ZJ-30 · pstack 吸收讨论](https://linear.app/zjlinjie/issue/ZJ-30)：本次仅落实 How 与上述规则增强，其余候选不在范围内。

原任务单中的 `$feature`、`$debug` 已按本次讨论更名；本文件记录确认后的实现范围，不改动 Linear 原任务单。
