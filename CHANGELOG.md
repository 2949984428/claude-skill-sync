# Changelog

All notable changes to this plugin will be documented here.

## [0.2.0] - 2026-06-25

### 新增

- **按用量排序 Skill 列表**：解析 Claude Code 会话日志（`~/.claude/projects/**/*.jsonl`），统计每个 Skill 的实际调用次数，侧边栏可按调用次数从高到低排序，一眼看出哪些 Skill 真正在用、哪些只是占位
- **用量时间范围**：可选统计全部历史或最近 N 天（设置项 `usageRange`），结果缓存 60 秒避免重复扫描
- Skill 名自动去掉插件命名空间前缀（如 `superpowers:brainstorming` → `brainstorming`），与 vault 内裸目录名对齐

### 优化

- 点击 Skill 名打开 SKILL.md 的逻辑更稳健（兼容小写 `skill.md`）
- 侧边栏与状态卡样式打磨

## [0.1.0] - 2026-04-23

### 首发版本

**核心功能**
- 扫描 Obsidian 内的 Skill 文件夹，一键 symlink 到 18+ Coding 工具（Claude Code / Codex / Cursor / Gemini CLI / Windsurf / Trae 等）
- 双向同步：vault → 工具（安装）+ 工具 → vault（导入）
- 8 格同步状态卡：已同步 / 部分同步 / 未安装 / 待导入 / 指向错误 / 失效链接 / 冲突，每格 hover 显示用户视角的 tooltip
- Skill 列表带搜索 + 折叠 description

**自动化**
- 定时扫描（默认 5 分钟，可配置）
- 自动安装新 Skill 到所有 Coding 工具（默认开，安全）
- 自动从工具导入新 Skill 到 vault（默认关，需主动开启）
- 启动时同步状态摘要通知

**安全护栏**
- 移除只断开链接，不删除源 Skill
- 冲突状态需人工处理，绝不覆盖工具自带的实体 Skill
- 删除按钮二次确认（3 秒确认窗口）

**配置**
- 添加 Coding 工具弹窗：18 个预设可选 + 自定义路径
- 设置面板用用户视角语言，避免技术黑话
- 仅桌面端（移动端 Obsidian 不支持 symlink）
