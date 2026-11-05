# AI 动物寓言短剧 · AI Fable Series

[中文](#中文) · [English](#english)

---

## 中文

**一只话少的老乌鸦 + 一只爱跟风的胖仓鼠**：豆豆捧着一大碗麦片说「早餐可是一天最重要的一餐！」，墨先生抬眼看他，不说话，然后问：「谁告诉你的？」「大家都这么说啊。」「那大家，又是听谁说的？」豆豆愣住，墨先生把羽毛尖竖在喙前，镜头切到一台 1944 年的老收音机……

外网有一类号用拟人动物拆穿日常习惯和广告套路（一天一万步、早餐最重要、咖啡时间），几个月涨到几百万粉。这个仓库是做这种 **AI 动物寓言短剧** 的完整流程，免费开源：说一个话题，就查证、写剧本、拆段、出全部提示词。

- **Claude Skill**：装进 [Claude Code](https://claude.com/claude-code) 后说「出一集：一天真的要喝八杯水吗？」
- **固定的开头公式**：小角色说出大家都信的那句话 → 智者先沉默 → 问两次「谁告诉你的」「那大家又是听谁说的」→ 小角色愣住 → 智者做「嘘」手势 → 画外音摆证据
- **先查证**：说的是真公司、真事，每句都要一手出处（法院文书、监管公告、当年的广告原件、论文），分清事实和指控，查不到的不写
- **提示词自动拼**：按 30 秒一段拆好；每段一张 2D 动画分镜（16:9 宫格、每格竖构图、不带序号、每格标人数）+ 一份 Seedance 视频提示词（9:16，参考图编号、台词、画外音不对口型、秒数自动对齐）
- **改一格、补几秒**：分镜某一格、视频某几个镜头不对，只重做那一点

> 还没有实拍演示。做出来的第一条欢迎发给我，我放到这里。

### 做法

| | 是什么 | 在哪做 |
| --- | --- | --- |
| ① 设定图 | 两个角色（白底三视图 + 脸部特写 + 表情 + 招牌手势）+ 固定场景（两个角度） | 即梦画布 · 图片生成 |
| ② 分镜图 | 每 30 秒一张 2D 动画八宫格 | 即梦画布 · 图片生成（框选设定图） |
| ③ 视频 | 设定图 + 分镜 + 按秒写的镜头、台词、音效，一次 30 秒，9:16 | 即梦画布 · Seedance 2.5 全能参考 |
| ④ 拼接 | 几段拼成一条，铺音乐、加字幕和封面大字 | 脚本 + 剪映 |

### 时长

| 时长 | 段数 | 怎么排 |
| --- | --- | --- |
| 30 秒 | 1 | 开头公式压到 4 个镜头，剩下讲证据和结尾 |
| 60 秒（默认） | 2 | 第 1 段开头公式，第 2 段证据、结果、结尾 |
| 90 秒 | 3 | 中间多一个证据或一个反方向的说法 |

### 例子

[`examples/breakfast/`](examples/breakfast/)：《早餐是一天最重要的一餐？》，60 秒 2 段，[剧本](examples/breakfast/ep01/剧本.md)（带出处表）、[全部提示词](examples/breakfast/ep01/提示词.md)、[设定图提示词](examples/breakfast/设定图提示词.md)。

默认角色是老乌鸦墨先生 + 胖仓鼠豆豆，场景是街角旧书店；[`docs/01-characters.md`](docs/01-characters.md) 里还有两组（老山羊 + 浣熊、猫头鹰 + 兔子），也可以自己定。

### 你需要

- 即梦会员（画布 + Seedance 2.5）；海外用 Dreamina，提示词通用
- 跑脚本：Python 3.9+、ffmpeg、`pip install pillow`

### 安装成 Claude Skill

把这个仓库的链接丢给 Claude Code，说「帮我装成 Skill」。或者：

```bash
git clone https://github.com/10291124jcjc-dotcom/ai-fable-series.git ~/.claude/skills/ai-fable-series
```

### 只用脚本

```bash
python scripts/setup_check.py my-series ep01                # 检查环境、建文件夹、复制模板
python scripts/make_prompts.py my-series sheets             # 角色、固定场景的设定图提示词
python scripts/make_prompts.py my-series ep01 script        # 剧本（含出处表）
python scripts/make_prompts.py my-series ep01               # 全部分镜 + 视频提示词，一份文件
python scripts/make_prompts.py my-series ep01 panel 7 --seg 1   # 只重出第 1 段分镜的第 7 格
python scripts/make_prompts.py my-series ep01 patch 6-7 --seg 1 # 只补拍第 1 段的镜头 6–7
python scripts/review_clip.py my-series ep01                # 抽帧检查生成的视频
python scripts/compile.py concat 成片.mp4 第1段.mp4 第2段.mp4
```

### 注意

- 只做原创角色：不照搬别人账号的角色、台词和封面字，只学结构
- 说真公司、真事要有一手出处；被起诉不等于有罪，和解不等于认错
- 画面里不画真实商标；健康话题不给治疗建议
- 配乐用你自己有版权的；发布时标注 AI 生成

### 致谢

开头公式（沉默 → 问两次 → 「嘘」 → 画外音讲证据）参考了 YouTube 频道 Ai Playkit 公开分享的提示词结构；脚本和 [ai-science-series](https://github.com/10291124jcjc-dotcom/ai-science-series) 是同一套。

### 作者

硅基废话（X：[@DDJCXX](https://x.com/DDJCXX)）。做出来的视频欢迎 @ 我。

---

## English

**A quiet old crow and an eager hamster.** The hamster repeats something everyone believes ("Breakfast is the most important meal of the day!"). The crow stays silent, then asks twice: "Who told you that?" "Everyone says so." "And who told everyone?" The hamster freezes, the crow raises a feather to his beak, and the narration cuts to the evidence: a 1944 cereal ad campaign, a 2019 BMJ meta-analysis.

This repo is the full workflow for this kind of **AI animal fable short**, packaged as a Claude Skill:

- A fixed opening beat (claim → silence → two questions → "shh" → evidence in voiceover)
- Research first: every claim about a real company or habit needs a primary source; allegations are kept separate from facts
- Auto-generated prompts per 30-second segment: a 2D storyboard grid (no numbers, cast count per panel) and a Jimeng / Dreamina **Seedance 2.5** omni-reference video prompt (9:16, reference numbering, dialogue, voiceover without lip-sync, timing)
- Patch a single panel or a few seconds instead of regenerating a whole segment

No demo video yet. Example episode: [`examples/breakfast/`](examples/breakfast/). Dialogue defaults to Mandarin; change `lang_note` in `series.json` and write English lines for an English series.

Install: `git clone https://github.com/10291124jcjc-dotcom/ai-fable-series.git ~/.claude/skills/ai-fable-series`

License: scripts MIT; docs, examples, templates CC BY-NC 4.0 (see [LICENSE](LICENSE)).
