<div align="center">

# 剧有词（增强版）

### 看一集，学几句。

给出美剧剧名和集数，把这一集的词库加入你的通用 HTML 学习器。

[下载体验页](examples/demo.html) · [安装](#安装) · [使用方法](#使用)

![MIT License](https://img.shields.io/github/license/Chase-qisiai/juyouci?style=flat-square)
![通用学习器](https://img.shields.io/badge/output-reusable%20HTML-235a47?style=flat-square)
![无需 Anki](https://img.shields.io/badge/Anki-not%20required-687772?style=flat-square)

</div>

> 本仓库是 [Chase-qisiai/juyouci](https://github.com/Chase-qisiai/juyouci) 的增强版，保留原 MIT 许可与上游版权声明。新增功能见下方「✨ 新增功能」。

## ✨ 新增功能（相对原版）

### 1. 词根词缀解析（morph 字段）

每条**英语**词卡可带结构化词根词缀解析，在卡片上分区展示：

```
re-           view
前缀 Prefix  词根 Root
再、又        看
re-（再）+ view（看）→ 再看一遍，复习
```

- 数据结构：卡片新增可选 `morph` 字段，含 `prefix` / `root` / `suffix` 三个分区（各为 `{"part": 词缀, "meaning": 中文含义}`），另有可选 `note` 整词串联说明
- 只标注确实可判断的构词成分；短词、借词或拼写演变无法可靠拆分时省略，不强行拆分、不编造词源
- 非英语词条（日语、韩语、中文等）一律不填

### 2. 词根词缀选词偏好

内置 `references/favorite-affixes.md` 默认词根词缀清单（约 40 个前缀、90 个词根、30 个后缀，均带中文含义与示例词）。整理词库时，在实用度和难度相当的前提下，**优先挑选清单中的构词词**，方便按构词法记忆。清单可随个人偏好随时增补。

### 3. 朗读单词按钮

学习卡片上除「朗读原句」外，新增**「朗读单词」**按钮（学习阶段与核对答案阶段各一组），可单独听词头发音。

### 4. 难度级别可指定

默认按 CEFR B2–C1 选词，但**生成前直接指定级别即可覆盖**，例如：

```
用剧有词生成《The Pitt》S01E01 词库，词汇级别控制在 CEFR A1–B1
```

生成后可回复「太难/太简单」逐档调整；支持 CET-4/6、IELTS、TOEFL、JLPT、TOPIK、HSK 等考试范围。

## 学习流程

```
flowchart LR
 A[第一次：剧名 + 集数] --> B[核实字幕并筛选词汇]
 B --> C[创建通用学习器并加入首集]
 C --> D[~/Documents/series-vocab/juyouci.html]
 E[后续：新的剧名 + 集数] --> F[核实字幕并整理新词库]
 F --> G[合并回同一个 HTML]
 D --> H[选择剧集词库]
 G --> H
 H --> I[翻卡自评]
 H --> J[可选：输入回忆]
 J -->|答错| K[自动重练]
 K --> J
```

每条词汇卡包括目标词、音标、语境释义、原句和译文，英语词条还带词根词缀解析。翻卡自评是默认方式；输入回忆会隐藏原句中的目标表达，检查输入并安排错词重练。每个剧集词库独立保存练习进度。

剧集原语言决定词汇语言：日剧提取日语，韩剧提取韩语，中文剧提取中文，其他语言同理。默认按目标语言的常见中高级范围选词；如果用户说明考试和级别，会优先按对应考试范围筛选，例如 CET-4/CET-6、IELTS、JLPT、TOPIK、HSK/TOCFL。

## 安装

支持 Skills 的 Agent（Codex、Doubao 等），把整个仓库放入它的个人 Skills 目录即可：

```bash
git clone --depth 1 https://github.com/<你的账号>/<本仓库>.git ~/.codex/skills/juyouci
```

已安装时进入技能目录执行 `git pull` 更新。安装后在新任务里这样使用：

```
使用 $juyouci，为我生成 The Pitt S01E01 的 HTML 词汇学习页。
```

也可以直接把字幕或剧本文字提供给 Agent。

## 使用

默认每集整理最多 50 条中文词汇，字幕材料不足时按实际内容生成，不凑数量。已有字幕会直接使用。

```
用剧有词生成《The Pitt》S01E01 词库，加入我的学习器。
```

首次使用时，学习器保存在 `~/Documents/series-vocab/juyouci.html` 并包含首集词库。以后提出新剧集，Agent 会把新词库自动合并回同一个文件，交付更新后的 HTML 并说明已加入哪一集。若 Agent 找不到已有学习器或无法读取它，会向你索要原文件路径或文件，不会另建一个空库覆盖原有内容。

双击 `juyouci.html` 即可学习。网页不依赖 Anki、服务器或网络；无需安装 Python。页面默认使用翻卡自评，可在页面中切换「输入回忆」。HTML 文件包含整套词库，复制它即可备份词库；练习进度和模式偏好保存在浏览器，需要用页面的进度备份功能另行导出。

每次生成后请留意 Agent 的难度回访。回复「太难」或「太简单」，并告诉它你要考的考试和级别，Agent 会更新同一剧集词库。

## 手动生成

需要 Python 3 的标准库来校验词库并合并到通用学习器；学习者只需浏览器。

```bash
python3 scripts/build_html.py examples/demo.json study.html
```

词库数据格式见 `references/data-format.md`，词根词缀清单见 `references/favorite-affixes.md`。

## 项目结构

| 路径 | 用途 |
|---|---|
| `SKILL.md` | Agent 的触发条件与生成流程（含词根词缀规则） |
| `assets/study.html` | 通用学习器模板（含词根词缀展示与朗读单词按钮） |
| `scripts/build_html.py` | 校验词条并生成学习页（支持 morph 字段） |
| `references/favorite-affixes.md` | 词根词缀默认清单（选词偏好） |
| `references/` | 字幕来源与词条格式说明 |
| `examples/demo.html` | 可直接打开的原创示例 |

## 维护者验证

```bash
python3 -m unittest discover -s tests -p 'test_*.py'
npm install && npm test
```

浏览器交互测试使用本机 Chrome 和 Playwright。

## 来源与许可

本项目由 [pyang5166/gbro-series-vocab](https://github.com/pyang5166/gbro-series-vocab) 改编，将 Anki/Markdown 交付改为独立 HTML 学习页，并在 [Chase-qisiai/juyouci](https://github.com/Chase-qisiai/juyouci) 基础上增强词根词缀解析与朗读功能。保留上游 MIT 许可和原作者版权声明。
