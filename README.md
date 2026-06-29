# 问心 (Wen-Xin)

**输出即验证。你的回复本身就是验证凭证。**

**作者**: XP
**主页**: [github.com/xp1001/wen-xin](https://github.com/xp1001/wen-xin)

问心是一个 OpenCode meta-skill，核心原则是**输出即验证**——你的回复中必须包含精确覆盖和抽样核验的证据，不在对话中增加额外的"验证步骤"。

## 核心机制

### 检查清单

每条对应一个输出要素：

**必须融入输出**（每次回复中自然体现）：

| 行动 | 怎么做 |
|------|--------|
| **精确覆盖** | 需求逐条列出，每条标注覆盖状态。说出精确数量 |
| **抽样核验** | 头、中、尾各抽一条，逐字段检查，在回复中提及抽查结论 |

**异常才说**（正常时不提，有问题时才在回复中提及）：

| 行动 | 怎么做 |
|------|--------|
| **边界触发** | 构造异常场景（空数据、断网、格式错），确认有处理 |
| **重跑凭证** | 验证命令再跑一次，确认当前能过 |

**内部保留**（仅用于自省，不输出）：

| 行动 | 怎么做 |
|------|--------|
| **必要性标注** | 标记自行添加的功能，判断是否必要 |
| **诚实标记** | 回顾以上，标记模糊地带 |

### 修正循环

发现问题 → 当场修复 → 重新验证。只有「已完成」才能退出。

### 判定

| 状态 | 含义 |
|------|------|
| 已完成 | 精确覆盖和抽样核验已在输出中体现，其余项无存疑 |
| 基本完成（有瑕疵） | 核心目标达成，但有次要问题 |
| 未完成 | 核心目标未达成或发现实质性 Bug |

## 安装技能

所有兼容工具都读取 `~/.claude/skills/` 或 `~/.agents/skills/`，克隆一次即可处处生效：

```bash
git clone git@github.com:xp1001/wen-xin.git ~/.claude/skills/wen-xin
# 或
git clone git@github.com:xp1001/wen-xin.git ~/.agents/skills/wen-xin
```

自动生效于：**OpenCode / Claude Code / Cline / Roo Code / Augment / Codex / Workbuddy**

## 强制触发

技能默认是"按需加载"(agent 根据描述决定是否启用)。要让问心**每次都跑**，需在全局规则文件中加入强制指令。

### 规则内容（所有平台通用）

```
## 问心
每次声称完成或汇报前，必须调用 wen-xin skill 执行验证。铁律，不可跳过。
```

### 各平台规则文件位置

| 平台 | 规则文件 | 说明 |
|------|---------|------|
| **OpenCode / Workbuddy** | `~/.config/opencode/AGENTS.md` | 全局生效 |
| **Claude Code / Cline / Roo** | 项目 `CLAUDE.md` 或 `~/.claude/CLAUDE.md` | 项目或用户级 |
| **Cursor** | 项目 `.cursorrules` 或 `.cursor/rules/*.mdc` | 项目级 |
| **Windsurf** | 项目 `.windsurfrules` | 项目级 |
| **OpenClaw** | `~/.openclaw/workspace/SOUL.md` | 用户级 |
| **GitHub Copilot** | `.github/copilot-instructions.md` | 项目级 |
| **Augment Code** | 项目 `AGENTS.md` | 自动读取 |
| **其他** | 项目中创建 `AGENTS.md` 或 `CLAUDE.md` | 通用方案 |

### 一句话法

不支持 skill 系统的工具，直接把 `SKILL.md` 全文复制到规则文件中即可。

### 触发词

prompt 中以 `wen-xin` 开头可主动唤醒并查看完整报告：

> wen-xin，实现一个排序函数...

## 验证测试

用 3 个场景验证了问心效果：

| 场景 | 结果 |
|------|------|
| 完整任务（冒泡排序） | ✅ 正确判定「已完成」，8 条全部通过 |
| 遗漏日志（排序无日志） | ✅ 发问发现遗漏 → 自动补日志 → 再验证通过 |
| NaN Bug（去重函数） | ✅ 发问发现 Bug → 自动修复 → 测试通过 |

## 项目结构

```
wen-xin/
├── SKILL.md    # 技能定义（触发条件 + 执行流程）
└── README.md
```

## License

MIT
