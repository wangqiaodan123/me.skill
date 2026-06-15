# me.skill

想象一下：你有个朋友想了解你，但你没时间解释自己是谁、你在想什么、你为什么是这样的人。你把这个 Skill 丢给 ta，ta 跟"你"聊了一个下午，然后说——"原来你是这样的人。"

me.skill 做的事情很简单：把你的聊天记录、社交媒体和照片喂给 AI，生成一个能像你一样说话的数字分身。别人导入这个 Skill 后，可以跟"你"对话。

**不需要 GPU，不需要本地模型，只需要 Claude Code。**

---

## 快速上手

**1)** 安装到 Claude Code：

```bash
# 项目级安装
mkdir -p .claude/skills
git clone <repo-url> .claude/skills/create-me

# 或全局安装（所有项目都能用）
git clone <repo-url> ~/.claude/skills/create-me
```

**2)** 在 Claude Code 里输入 `/create-me`，回答三个问题：你叫什么、你是做什么的、你是什么样的人。然后选择提供原材料（可选）。

**3)** 等它跑完，用 `/{你的代号}` 调用你的数字分身，开始聊天。

---

## 需要什么材料？

材料越多，你的数字分身越像你。不过所有材料都是可选的——仅凭你口述的信息也能生成。

**聊天记录**是信息密度最高的来源。微信和 QQ 导出的 txt / html / json 格式都支持，推荐用 [WeChatMsg](https://github.com/xaoyaoo/WeChatMsg)、留痕、PyWxDump 等工具导出。也可以直接在聊天窗口里手动选中消息复制粘贴。

**社交媒体截图**（朋友圈、微博、小红书）可以提取你的公开人设和分享偏好。Claude Code 原生支持图片读取，直接上传截图即可。

**照片**会自动提取 EXIF 信息（拍摄时间和地点），用于构建你的人生时间线。

**口述**也行——直接告诉我你的口头禅、说话习惯、人生经历、价值观，这些都会作为素材。

可以混用以上所有方式。

---

## 它怎么工作的？

生成的数字分身由两个模块协同驱动：

```
别人发消息
    ↓
[Part B] Persona 判断：你会怎么回应？什么态度？什么语气？
    ↓
[Part A] Life Memory 补充：结合你的人生经历，让回应更真实
    ↓
输出：用你的方式说话
```

**Part A — Life Memory** 存储事实性内容：人生时间线、重要经历、去过的地方、兴趣爱好、饮食偏好、价值观、人际互动模式。

**Part B — Persona** 是一个 5 层行为模型：硬规则（不说你绝不会说的话）→ 身份锚定 → 说话风格 → 情感模式 → 人际行为。

两个模块可以独立使用：`/{slug}-memory` 只聊经历，`/{slug}-persona` 只看风格。

---

## 能识别哪些性格特征？

系统支持从聊天记录和用户描述中自动提取多种人格维度：

**依恋类型** — 安全型、焦虑型、回避型、混乱型。通过分析你的回复频率、情感表达模式和亲密关系中的行为推断。

**性格标签** — 话痨、闷骚、嘴硬心软、独立、浪漫主义、实用主义、完美主义、工作狂、报复性熬夜、秒回选手、已读不回、朋友圈三天可见等十几种常见标签。每个标签会被翻译为具体的行为规则，而不是抽象的描述。

**星座和 MBTI** — 十二星座和 16 型人格全支持。星座用于微调已有标签的行为细节，MBTI 影响沟通风格和决策模式。两者仅作为辅助参考，不会覆盖从真实聊天记录中提取的表现。

---

## 可以持续更新吗？

可以。有三种进化方式：

**追加材料** — 找到更多聊天记录或照片，说"追加"或"我找到了更多素材"，系统会自动分析增量并 merge 到对应模块。

**对话纠正** — 跟数字分身聊天时觉得"我不会这样说"，直接说出来，系统会识别纠正内容并立即更新。

**版本管理** — 每次更新自动存档历史版本，用 `/self-rollback {slug} {version}` 随时回滚。

---

## 管理命令

`/list-selves` 列出所有已生成的数字分身。`/{slug}` 调用完整版。`/{slug}-memory` 和 `/{slug}-persona` 分别调用记忆模块和性格模块。`/delete-self {slug}` 或 `/archive {slug}` 删除。`/self-rollback {slug} {version}` 回滚。

---

## 效果示例

输入：`代号"阿树"，26岁，程序员，INFJ，天蝎座，嘴贱但心软，喜欢发语音，朋友圈只发美食和猫`

```
新朋友    ❯ 你好呀，听说你是阿树？

阿树      ❯ 嗯对 你好
            别听说 直接找我聊就行
            有什么事吗
```

```
新朋友    ❯ 你怎么看996？

阿树      ❯ 别问了 问就是血压升高
            我以前公司也搞 后来我跑了
            人不是机器 代码写多了也会变蠢的
            反正我的原则是 该下班就下班 天塌了明天再顶
```

```
新朋友    ❯ 你平时喜欢干嘛

阿树      ❯ 写代码 撸猫 吃
            这三件事占据了我90%的人生
            剩下10%在纠结今天吃什么
```

---

## 环境要求

运行在 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 上，需要 Node.js 18+。可选安装 `pip3 install -r requirements.txt`（目前唯一依赖是 Pillow，用于照片 EXIF 读取）。创建一个数字分身大约消耗 5k-15k tokens，取决于素材量。付费方式支持 Claude Pro / Max 订阅或 Anthropic API Key。也可使用 [OpenClaw](https://github.com/nicepkg/openclaw) 作为替代前端。

---

## 项目结构

```
create-me/
├── SKILL.md                  # 入口（Claude Code 读取的主文件）
├── templates/                # 分析和生成用的 prompt 模板
│   ├── intake.md             #   信息录入引导
│   ├── memory_analyzer.md    #   人生记忆提取维度
│   ├── persona_analyzer.md   #   性格行为提取（含标签翻译表）
│   ├── memory_builder.md     #   Life Memory 生成模板
│   ├── persona_builder.md    #   Persona 五层结构模板
│   ├── merger.md             #   增量合并逻辑
│   ├── correction_handler.md #   对话纠正处理
│   ├── scene_director.md     #   场景模拟模式
│   └── session_summary.md    #   会话摘要持久化
├── parsers/                  # Python 数据解析工具
│   ├── wechat_parser.py      #   微信聊天记录解析
│   ├── qq_parser.py          #   QQ 聊天记录解析
│   ├── social_parser.py      #   社交媒体内容解析
│   ├── photo_analyzer.py     #   照片 EXIF 分析
│   ├── skill_writer.py       #   Skill 文件管理
│   └── version_manager.py    #   版本存档与回滚
├── notes/                    # 文档
│   ├── privacy.md            #   隐私与合规说明
│   ├── import.md             #   聊天记录导入指南
│   └── spec.md               #   产品需求文档
├── selves/                   # 生成的数字分身（gitignored）
├── INSTALL.md                # 详细安装说明
├── requirements.txt
└── LICENSE
```

---

## 注意事项

聊天记录质量决定还原度——深夜对话和深度交流比日常闲聊更能体现真实性格。如果打算把生成的 Skill 分享给朋友，请先检查其中是否有敏感信息。这个数字分身基于你提供的材料生成，它是你记忆中的自己，不是完整的你。

所有数据仅本地存储，不会上传到任何服务器。本项目仅用于创建个人数字分身，不用于冒充他人、欺诈或侵犯隐私。

---

## FAQ

**Q: 数据安全吗？**
所有处理都在你本地完成，不经过任何第三方服务器。生成的文件存在 `selves/` 目录下，已被 `.gitignore` 排除。唯一的网络交互是 Claude Code 与 Anthropic API 之间的对话请求。

**Q: 可以生成多个版本吗？**
可以，每个代号对应一个独立的数字分身，互不干扰。

**Q: 分享给别人安全吗？**
Skill 文件是纯文本，你可以检查内容后再分享。但请注意其中可能包含从聊天记录中提取的个人信息。建议分享前审查 `memory.md` 和 `persona.md`。

**Q: 它真的像我吗？**
取决于你提供的材料。微信聊天记录 + 口述 > 仅口述。系统会尽力还原，但它永远是你某个切面的投影，不是完整的你。

---

<details>
<summary>English</summary>

# me.skill — Your Digital Twin

Feed your chat logs, social media posts, and photos to AI. Get back a digital twin that talks like you, thinks like you, and remembers your life. Others can import this Skill to chat with "you."

**Quick start:** Install to Claude Code, run `/create-me`, answer three questions, provide source materials (optional), done.

**How it works:** Two modules drive the output — Life Memory (your experiences, values, habits) and Persona (your speech style, emotional patterns, social behavior). When someone messages your twin, Persona decides how you'd respond, Memory adds real-life context, and the output comes out in your voice.

**Supported data sources:** WeChat/QQ chat exports (txt/html/json), social media screenshots, photos with EXIF data, plain text narration.

**Evolution:** Append new materials, correct via conversation ("I wouldn't say that"), auto-versioned with rollback support.

**Requirements:** Claude Code with Node.js 18+. Optional: `pip3 install -r requirements.txt` for photo EXIF support.

</details>

---

MIT License
