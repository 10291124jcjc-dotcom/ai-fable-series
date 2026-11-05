---
name: ai-fable-series
description: AI 动物寓言短剧：一只安静的智者动物 + 一只爱跟风的小动物，用「小角色说出大家都信的一句话 → 智者先沉默 → 问两次『谁告诉你的』『那大家又是听谁说的』→ 小角色愣住 → 智者做『嘘』手势 → 画外音摆证据」的固定开头，拆穿日常习惯和营销套路背后的来历（早餐最重要、一天一万步、钻石恒久远……）。复古手绘 2D 动画、竖屏 9:16，用户说一个话题，就先查一手出处，再写剧本、按 30 秒一段拆好，自动拼出角色设定图、宫格分镜图和即梦 Seedance 全能参考视频提示词，用户在即梦画布里出图出视频，再检查、补拍、拼接。用户说「做一个像西装狼那样的号」「Quiet Wolf 同款」「动物讲营销套路」「拆穿广告的动画」「AI 寓言短剧」「出一集 XX」，或者要改已有一集的剧情、镜头、提示词时使用，哪怕没提 Skill 名字。Make a two-animal 2D fable series that debunks everyday habits and marketing myths, with Jimeng (Dreamina) Seedance omni-reference.
---

# AI 动物寓言短剧

固定两个原创拟人动物：一个话少、笃定的智者，一个热心、爱跟风的小角色。每集拆一句「大家都这么说」的话：它最早是谁、为了卖什么喊出来的，研究又怎么说。复古手绘 2D 动画，竖屏 9:16，默认 60 秒（2 段），在即梦画布里一段一段生成再拼起来。

## 0. 先说清楚

- 用中文和用户说话。给用户复制的提示词不加多余说明。
- **只用即梦画布**：图和视频都在即梦画布里生成。你负责选题、查证、写剧本、拼提示词、检查，生成那一步用户自己点。海外用 Dreamina，提示词通用。视频先出 480p，满意再超清。
- **事实必须有一手出处**：说的是真公司、真事，查不实就是造谣。法院文书、监管公告、当年的广告原件、论文；二手文章只当线索。分清事实和指控，不把话说死。见 `docs/02-research.md`。
- **原创**：这个格式来自外网「穿西装的狼 + 戴礼帽的猪」那类号，只学结构。不做狼 + 黑西装 + 红领带、猪 + 圆顶礼帽，不搬别人的台词和封面字；画面里不画真实商标。见 `docs/01-characters.md`。
- 路径：`S/` = 本 Skill 的 `scripts/`；`SERIES` = 用户的系列文件夹。脚本和 `ai-science-series` 是同一套，改成了 2D 动画和竖屏。

## 1. 第一次：建系列

1. `python S/setup_check.py SERIES`：检查 Python、ffmpeg、Pillow，建好 `refs/`、`log.md`，复制 `templates/series.json`（默认角色：老乌鸦墨先生 + 胖仓鼠豆豆，场景是街角旧书店）。
2. 按 `docs/01-characters.md` 定角色。用户没想法就给那里的三组让他挑，或者照默认。改 `series.json`：`look`、`short`、`voice`、招牌手势（写进 `poses` 第二个）。发海外就把台词改英文、改 `lang_note`。
3. `python S/make_prompts.py SERIES sheets` → `SERIES/设定图提示词.md`。用户在即梦里出图，存进 `refs/`。
4. 用 Read 逐张看设定图：同一个角色前后一致、没有多手多脚、「嘘」手势画对了。不对就让用户重出。

## 2. 每一集

1. **选题和查证**（`docs/02-research.md`）：先读 `log.md` 避开讲过的。一句人人都信的话 + 能查到的起源 + 一个反常识的结果。每句事实写进 `episode.json` 的 `facts`（说法、出处、注意）。
2. **剧本**（`docs/03-script.md`）：第 1 段照开头公式 8 拍走；后面是证据（一句台词配一个 B-roll 画面，智者用画外音）→ 回到场景总结源头 → 研究数字 → 小角色回扣手里的东西 → 智者一句不说死的话 → 再做一次「嘘」，回扣开头那句话。
3. **拆段**：每段 30 秒、8 个镜头，每个镜头至少 2 秒，沉默镜头 3 到 3.5 秒。在两段以上出现的 B-roll 场景写进 `episode.json` 的 `scenes` 并带 `ref`。
4. 写好 `SERIES/epNN/episode.json`（格式见 `templates/episode.json`，完整例子见 `examples/breakfast/ep01/episode.json`），`python S/make_prompts.py SERIES epNN script` 生成 `剧本.md` 给用户看，改到他点头。
5. `python S/make_prompts.py SERIES epNN` → `epNN/提示词.md`：每段一张 2D 动画分镜图提示词（16:9 宫格，每格竖构图，不带序号，每格标人数）+ 一份 Seedance 视频提示词（9:16，参考图编号、台词、画外音不对口型、不要背景音乐都自动写好）。脚本会检查秒数、语速、参考图上限。
6. 用户按 `docs/04-jimeng-canvas.md` 在画布里操作：每段先出分镜，再框选参考图 + 分镜出视频。第一集先做第 1 段，开头公式对了再做后面。
7. **检查**：视频放进 `epNN/clips/第N段.mp4`，`python S/review_clip.py SERIES epNN`，用 Read 看抽帧：人物有没有变样或重复、沉默镜头有没有乱张嘴、「嘘」手势对不对、有没有乱码字和商标。声音请用户听。
8. **拼接**：`python S/compile.py concat epNN/成片.mp4 epNN/clips/第1段.mp4 ...`。背景音乐、字幕、封面大字（`cover`）在剪映里加，字幕对照 `剧本.md` 校对年份和数字。
9. 写 `log.md`：话题、用过的 B-roll 物件、用过的出处、下期选题。

## 3. 改一个情节

- 分镜某一格不对：`python S/make_prompts.py SERIES epNN panel 6 --seg 1` 拿单格提示词，只重出那一格，用 `grid.py split` / `grid.py grid` 换回去。
- 某段几个镜头不对：`python S/make_prompts.py SERIES epNN patch 6-7 --seg 1` 拿补拍提示词，生成后 `compile.py replace` 换掉那几秒。
- 剧情要改：改 `episode.json` 后重跑 `script` 和提示词，只重做改到的那几段。

## 4. 踩坑

见 `docs/05-pitfalls.md`：分镜画出序号、同一格两个角色、沉默镜头乱张嘴、画外音被对口型、「嘘」手势画错、画出商标、画风越做越像别人的号。
