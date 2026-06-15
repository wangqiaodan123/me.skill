# 详细安装说明

## Claude Code 安装

### 项目安装

在你的 git 仓库根目录执行：

```bash
mkdir -p .claude/skills
git clone https://github.com/therealXiaomanChu/me-skill .claude/skills/create-me
```

### 全局安装

```bash
git clone https://github.com/therealXiaomanChu/me-skill ~/.claude/skills/create-me
```

### OpenClaw 安装

```bash
git clone https://github.com/therealXiaomanChu/me-skill ~/.openclaw/workspace/skills/create-me
```

---

## 依赖安装

### 基础依赖（可选）

```bash
cd .claude/skills/create-me  # 或你的安装路径
pip3 install -r requirements.txt
```

目前唯一的可选依赖是 `Pillow`，用于读取照片 EXIF 信息。如果你不需要照片分析功能，可以跳过。

---

## 微信聊天记录导出指南
- 导出格式：txt / html / csv
- 使用方法：下载安装 → 登录微信PC版 → 选择联系人 → 导出

### 手动复制

如果以上工具都不方便，你也可以：
1. 在微信中打开你跟朋友的聊天窗口
2. 手动选择并复制关键对话
3. 粘贴到一个 txt 文件中
4. 在 `/create-me` 时使用方式 D（上传文件）

---

## QQ 聊天记录导出指南

1. 打开 QQ → 点击左下角 ≡ → 设置
2. 通用 → 聊天记录 → 导出聊天记录
3. 选择联系人 → 导出为 txt 格式
4. QQ的聊天记录也可以直接复制粘贴

---

## 常见问题

### Q: 数据会上传到云端吗？
A: 不会。所有数据都存储在你的本地文件系统中，不会上传到任何服务器。

### Q: 可以创建多个版本的自我 Skill 吗？
A: 可以。每个版本会生成独立的 `selves/{slug}/` 目录。

### Q: 创建后还能修改吗？
A: 可以。说"我不会这样说"触发对话纠正，或"我有新文件"追加原材料。每次修改都有版本存档，可以回滚。

### Q: 我想删除怎么办？
A: 使用 `/delete-self {slug}` 或 `/archive {slug}` 命令。
