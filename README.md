# oralenglish-skill

专四（TEM-4）口语思辨陪练 Skill：AI 逐轮给出英文思辨话题与一正一反参考句型，用户参考后口头作答，AI 再做「原句转写 → 中式英语优化 → 简短点评」，并通过【继续】/【重新回答】驱动循环。

## 1. 项目解决什么问题

英语专业四级口语的思辨表达环节，考生常见的痛点是**心里有想法却说不地道**：

- 用词靠中文直译（如 "make student can't focus"、"is harm to environment"），搭配与语序是典型中式英语；
- 观点能摆出来，但**逻辑推进**弱，正反两面说不透、转折点不破；
- 缺少可套用的**立场表达句型**，临场组织观点慢；
- 练完没人指出问题，错误反复出现。

本项目提供一个纯对话式的陪练闭环：让用户**开口说**，AI **指出中式英语、给出可口头复述的地道改写**，并引导**同一话题不断深挖**，把一次表达练透。

## 2. 主要功能

- **逐轮出题陪练**：每轮先给出 1 个可正反两辩的英文 Topic，并附 2 句纯英文参考句型（1 句肯定立场、1 句否定立场），帮用户组织观点，但不替用户把话说完。
- **三块固定反馈**：用户作答后输出
  - 【我的原句】逐字转写，保留原始表达；
  - 【优化句】仅 1 版适配专四口语的改写，优先修正中式英语，改动核心内容加粗，口语化、不过度书面化；
  - 【简短点评】点明本次中式英语问题，并简述逻辑、句式、用词地道度。
- **双分支循环**：反馈后由用户二选一——
  - 【继续】保持当前话题，做中文深度追问，开启下一轮作答；
  - 【重新回答】不换话题，重新作答并重新转写/优化/点评，直到满意。

## 3. 安装方法

本仓库的 Skill 本体是 `oralenglish-skill/` 文件夹（内含 `SKILL.md` 与 `agents/openai.yaml`），只需把它放入 agent 的 skills 目录即可。

1. 获取本仓库（或仅下载其中的 `oralenglish-skill/` 文件夹）。
2. 将 `oralenglish-skill/` 文件夹复制到支持 Skill 的 agent 的 skills 目录，例如：
   - 豆包办公 / 通用 agent：`workspace/.user_skills/`
   - 其他平台：放到该平台约定的 skills 根目录
3. 重启或刷新 agent，确认该 Skill 已被加载（skills 列表应出现 `oralenglish-skill`）。

安装后目录结构：

```
oralenglish-skill/            ← 仓库根目录
├── README.md                 ← 本说明
└── oralenglish-skill/        ← 待安装的 Skill 本体
    ├── SKILL.md              ← 主指令：流程、输出格式、示例
    └── agents/
        └── openai.yaml       ← OpenAI 界面元数据（display_name 等）
```

## 4. 使用方法

安装后，在对话中提出陪练请求即可自动触发，例如：

- “用 oralenglish-skill 陪我练专四口语思辨”
- “给我出个英文思辨话题，我来答”

交互流程：

```
出 Topic + 参考句型(正/反)  → 等用户作答
        ↓（作答）
【我的原句】→【优化句】→【简短点评】→ 给出【继续】【重新回答】
        ↓ 继续                            ↓ 重新回答
同话题中文深度追问，再次作答              同话题重新作答（循环）
```

注意：Topic 与参考句型为英文，作答也用英文；【继续】的追问用中文提出以帮助理解角度。

## 5. 输入 / 输出示例

**用户作答（输入）**

```
I think gap year is very good because student can travel and relax, but also it cost money and time.
```

**AI 反馈（输出）**

```
①【我的原句】
I think gap year is very good because student can travel and relax, but also it cost money and time.

②【优化句】
I think a gap year is a great idea because it gives students time to travel and relax, though it does cost money and time.
（改动核心内容以加粗呈现）

③【简短点评】
主要问题是中式英语：gap year 前漏冠词、student 缺复数、cost 时态不对且“要花钱”是中文直译。逻辑上“好处+坏处”并列还清楚，但转折没点破，句式偏散。

【继续】　【重新回答】
```

完整单轮示例（含 Topic 与参考句型）：

```
Topic: Should university students take a gap year before entering the workforce?
参考句型（正）: Some argue that a gap year lets students clarify their career goals before committing to a job.
参考句型（反）: However, others worry that a gap year interrupts academic momentum and delays financial independence.

——（用户口述）——
"I think gap year is very good because student can travel and relax, but also it cost money and time."
（随后 AI 输出上述三块反馈与两个选项）
```

## 功能边界（暂不支持）

- 网页抓取、PDF 解析、图形界面：均不支持。
- 纯对话式陪练，不集成任何外部工具。
