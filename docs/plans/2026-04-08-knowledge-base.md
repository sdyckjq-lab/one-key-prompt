# v2 激活钥匙知识库 实施计划

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** 为提示词炼金术 skill 创建精选知识库（50-80 个条目），并修改 SKILL.md 将知识库查询逻辑集成到炼金流程中。

**Architecture:** 三个 JSON 文件（people.json / theories.json / concepts.json）按类型分存储激活钥匙条目。SKILL.md 第二步炼金的 Agent prompt 中加入知识库查询和注入逻辑，实现"先查知识库，没命中再 AI 自由发挥"的混合模式。用户可在项目目录 `.prompt-alchemy/knowledge-base/` 下创建同名文件扩展知识库。

**Tech Stack:** JSON 知识库文件 + SKILL.md（Markdown prompt 工程文件）

**设计文档:** `docs/superpowers/specs/2026-04-08-knowledge-base-design.md`

---

### Task 1: 创建 knowledge-base 目录和 people.json

**Files:**
- Create: `knowledge-base/people.json`

**Step 1: 创建目录**

Run: `mkdir -p knowledge-base`

**Step 2: 创建 people.json — 人物类激活钥匙**

创建包含 ~25 个人物条目的 JSON 文件。每个条目严格遵循设计文档的字段定义。

条目清单（按场景覆盖优先）：

| id | name | 核心覆盖场景 |
|---|---|---|
| taleb | 塔勒布 | 分析、策略 |
| munger | 查理·芒格 | 分析、策略、学习 |
| kahneman | 丹尼尔·卡尼曼 | 分析、决策 |
| drucker | 彼得·德鲁克 | 策略、管理 |
| christensen | 克莱顿·克里斯坦森 | 策略、创新 |
| jung | 荣格 | 创意、写作 |
| adler | 阿德勒 | 沟通、学习 |
| maslow | 马斯洛 | 分析、策略 |
| murakami | 村上春树 | 写作、创意 |
| higashino | 东野圭吾 | 写作（叙事技巧） |
| pinker | 史蒂芬·平克 | 写作、沟通 |
| brooks | 弗雷德·布鲁克斯 | 编程 |
| raymond | 埃里克·雷蒙德 | 编程 |
| sunzi | 孙子 | 策略 |
| feynman | 费曼 | 学习、沟通 |
| jobs | 乔布斯 | 创意、沟通 |
| graham | 保罗·格雷厄姆 | 写作、编程 |
| thiel | 彼得·蒂尔 | 策略、创意 |
| seneca | 塞涅卡 | 写作、沟通 |
| aristotle | 亚里士多德 | 写作（说服）、分析 |
| demi | 德摩斯梯尼 | 沟通（演讲） |
| Carnegie | 卡内基 | 沟通 |
| kawakami | 川上量生 | 创意 |
| mcluhan | 麦克卢汉 | 分析、沟通 |
| gladwell | 马尔科姆·格拉德威尔 | 写作、分析 |

每个条目完整 JSON 格式：
```json
{
  "id": "taleb",
  "name": "塔勒布",
  "name_en": "Nassim Nicholas Taleb",
  "core_idea": "反脆弱——从混乱和不确定性中获益的能力",
  "scenarios": ["分析", "策略"],
  "keywords": ["不确定性", "风险", "黑天鹅", "反脆弱", "随机", "脆弱", "波动"],
  "prompt_template": "用塔勒布的反脆弱框架分析{topic}，找出哪些部分能从混乱中获益",
  "popularity": "high",
  "related": ["black-swan", "convexity"]
}
```

**质量要求：**
- `core_idea`：一句话，不超过 30 字
- `keywords`：至少 5 个，包含用户真实表达（非学术词汇）
- `prompt_template`：必须包含 `{topic}` 或 `{variable}` 占位符
- `popularity`：大部分为 `high`，少数小众但有独特价值者为 `medium`

**Step 3: 验证 JSON 格式**

Run: `python3 -c "import json; json.load(open('knowledge-base/people.json')); print('JSON valid')"`
Expected: `JSON valid`

**Step 4: Commit**

```bash
git add knowledge-base/people.json
git commit -m "feat: add people knowledge base with ~25 entries"
```

---

### Task 2: 创建 theories.json — 理论/模型类激活钥匙

**Files:**
- Create: `knowledge-base/theories.json`

**Step 1: 创建 theories.json**

条目清单：

| id | name | 核心覆盖场景 |
|---|---|---|
| porter-five-forces | 波特五力模型 | 分析、策略 |
| swot | SWOT 分析 | 分析、策略 |
| bcg-matrix | BCG 矩阵 | 策略 |
| blue-ocean | 蓝海战略 | 策略、创意 |
| first-principles | 第一性原理 | 分析、编程、创意 |
| occams-razor | 奥卡姆剃刀 | 分析 |
| fermi-estimation | 费米估算 | 分析 |
| system-dynamics | 系统动力学 | 分析、策略 |
| feedback-loop | 反馈回路 | 分析、编程 |
| prospect-theory | 前景理论 | 分析、策略 |
| bayesian-thinking | 贝叶斯思维 | 分析 |
| decision-tree | 决策树 | 分析、策略 |
| disruptive-innovation | 颠覆式创新 | 策略 |
| diffusion-curve | 扩散曲线 | 策略、分析 |
| design-thinking | 设计思维 | 创意、编程 |
| pyramid-principle | 金字塔原理 | 写作、沟通 |
| scqa | SCQA 框架 | 写作、沟通 |
| six-thinking-hats | 六顶思考帽 | 创意、分析 |
| pdca | PDCA 循环 | 学习、编程 |
| smart | SMART 原则 | 策略、学习 |
| pareto | 帕累托法则（80/20） | 分析、策略 |
| moore-crossing-chasm | 跨越鸿沟 | 策略 |
| jobs-to-be-done | JTBD（待办任务理论） | 分析、策略 |
| ansoff-matrix | 安索夫矩阵 | 策略 |
| cost-benefit | 成本效益分析 | 分析、策略 |

每个条目格式同 people.json，字段一致。

**Step 2: 验证 JSON**

Run: `python3 -c "import json; json.load(open('knowledge-base/theories.json')); print('JSON valid')"`

**Step 3: Commit**

```bash
git add knowledge-base/theories.json
git commit -m "feat: add theories knowledge base with ~25 entries"
```

---

### Task 3: 创建 concepts.json — 概念/隐喻类激活钥匙

**Files:**
- Create: `knowledge-base/concepts.json`

**Step 1: 创建 concepts.json**

条目清单：

| id | name | 核心覆盖场景 |
|---|---|---|
| confirmation-bias | 确认偏误 | 分析、学习 |
| survivor-bias | 幸存者偏差 | 分析 |
| dunning-kruger | 邓宁-克鲁格效应 | 学习、分析 |
| rubber-duck | 橡皮鸭调试 | 编程 |
| tech-debt | 技术债务 | 编程 |
| broken-windows | 破窗效应 | 编程、策略 |
| pomodoro | 番茄工作法 | 学习 |
| hook-body-cta | 钩子-主体-号召 | 写作、沟通 |
| mental-model | 心智模型 | 学习、分析 |
| compounding | 复利效应 | 策略、学习 |
| minimum-viable | 最小可行产品（MVP） | 策略、编程 |
| sandbox | 沙盒思维 | 创意、编程 |
| curse-of-knowledge | 知识诅咒 | 沟通、写作 |
| hanlon-razor | 汉隆剃刀 | 分析、沟通 |
| parkinsons-law | 帕金森定律 | 策略、学习 |
| law-of-triviality | 自行车棚效应 | 分析、策略 |
| goldilocks-rule | 金发姑娘原则 | 创意、分析 |
| inversion | 逆向思维 | 分析、创意 |
| second-order | 二阶效应 | 分析、策略 |
| map-territory | 地图非疆域 | 分析、学习 |
| ab-testing | A/B 测试思维 | 分析、编程 |
| jobs-framework | 用户故事思维 | 编程、分析 |
| rubber-band | 橡皮筋法则（拉伸成长） | 学习、创意 |
| parallel-universe | 平行宇宙法（假设翻转） | 创意、分析 |
| teach-back | 费曼教回法 | 学习、沟通 |

**Step 2: 验证 JSON**

Run: `python3 -c "import json; json.load(open('knowledge-base/concepts.json')); print('JSON valid')"`

**Step 3: Commit**

```bash
git add knowledge-base/concepts.json
git commit -m "feat: add concepts knowledge base with ~25 entries"
```

---

### Task 4: 修改 SKILL.md — 集成知识库查询逻辑

**Files:**
- Modify: `SKILL.md:47-170`（第二步炼金部分）

**Step 1: 在第二步炼金部分开头加入知识库查询说明**

在 `### 第二步：炼金（必须使用 Agent tool 并行调用）` 标题之后、`在一条消息中发起` 之前，插入知识库查询逻辑说明。

插入内容：
```markdown
**知识库查询（v2 新增）：**

在发起 Agent 调用之前，先查询本地知识库：

1. 读取知识库文件：
   - 内置路径：`knowledge-base/people.json`、`knowledge-base/theories.json`、`knowledge-base/concepts.json`（相对于 skill 所在目录）
   - 用户扩展路径：当前项目目录下的 `.prompt-alchemy/knowledge-base/` 同名文件
   - 合并规则：先读内置，再读用户扩展；同 id 条目用户版覆盖内置版；不同 id 合并

2. 匹配规则：
   - 从第一步诊断结果中提取：任务场景（scenarios）+ 用户输入中的关键概念
   - 对每个类型的知识库文件遍历所有条目
   - 评分：`score = keywords_overlap × 2 + scenario_match × 1`
     - `keywords_overlap`：用户输入文本与条目 keywords 的交集个数
     - `scenario_match`：诊断场景与条目 scenarios 有交集则为 1，否则为 0
   - 取每个类型评分最高的 top 3 条目

3. 结果处理：
   - 有命中条目 → 在对应 Agent prompt 中插入「参考知识库」字段（见下方模板）
   - 无命中条目 → Agent prompt 不变，纯 AI 自由发挥
```

**Step 2: 修改三个 Agent prompt 模板**

在每个 Agent prompt 的 `推荐规则：` 部分之后、`- 推荐 2-3 个候选` 之前，插入可选的知识库参考段落。

以 Agent 1（人物专家）为例，在 `推荐规则：` 后插入：

```markdown
{知识库参考，如果有命中条目则插入以下内容，否则删除整段：}
参考知识库（优先从中选择，也可以推荐知识库以外的）：
{逐条列出命中的条目，格式：}
- {name}：{core_idea}。适合场景：{scenarios 逗号分隔}。模板：{prompt_template}
```

Agent 2（理论专家）和 Agent 3（概念专家）做同样的修改。

**Step 3: 删除 v2 预留注释**

删除 SKILL.md 第 223 行的 HTML 注释：
```
<!-- v2 预留：Agent prompt 模板中可加入「参考知识库：{从 JSON 文件读取的高质量激活钥匙候选列表}」字段，供 v2 版本扩展 -->
```

**Step 4: Commit**

```bash
git add SKILL.md
git commit -m "feat: integrate knowledge base query into SKILL.md alchemy step"
```

---

### Task 5: 端到端手动测试

**Files:**
- 无新文件

**Step 1: 验证 JSON 文件完整性**

Run:
```bash
python3 -c "
import json, os
for f in ['people.json', 'theories.json', 'concepts.json']:
    path = f'knowledge-base/{f}'
    data = json.load(open(path))
    entries = data['entries']
    ids = [e['id'] for e in entries]
    # 检查必填字段
    for e in entries:
        assert all(k in e for k in ['id','name','core_idea','scenarios','keywords','prompt_template','popularity']), f'{e.get(\"id\",\"?\")} missing fields'
    # 检查 id 唯一
    assert len(ids) == len(set(ids)), f'{f} has duplicate ids'
    # 检查场景合法
    valid = {'写作','分析','策略','创意','编程','沟通','学习'}
    for e in entries:
        assert set(e['scenarios']) & valid, f'{e[\"id\"]} has no valid scenario'
    print(f'{f}: {len(entries)} entries, all valid')
"
```
Expected: 三个文件各输出条目数，全部 valid

**Step 2: 检查 related 引用完整性**

Run:
```bash
python3 -c "
import json
all_ids = set()
for f in ['people.json','theories.json','concepts.json']:
    data = json.load(open(f'knowledge-base/{f}'))
    all_ids.update(e['id'] for e in data['entries'])
# 检查 related 引用
for f in ['people.json','theories.json','concepts.json']:
    data = json.load(open(f'knowledge-base/{f}'))
    for e in data['entries']:
        for r in e.get('related', []):
            if r not in all_ids:
                print(f'Warning: {e[\"id\"]} references unknown id: {r}')
print('Cross-reference check complete')
"
```
Expected: 无 Warning，或少量可接受的跨文件引用 warning

**Step 3: 手动功能测试**

在 Claude Code 中调用 `/提示词炼金术`，输入测试提示词：

测试 1（应命中知识库）：
```
我想让你帮我分析一下我们公司面临的市场竞争情况，看看我们的优势和劣势在哪里，然后给我一些战略建议，要考虑竞争对手的反应，还要考虑行业发展趋势和新技术的影响。
```
预期：知识库应命中「波特五力模型」「SWOT」「蓝海战略」等条目，Agent 推荐中应包含这些。

测试 2（应命中编程相关）：
```
帮我写一段代码来处理用户输入的数据，数据格式不太确定，可能会有各种异常情况，需要做好错误处理和边界检查，代码要容易维护和扩展。
```
预期：知识库应命中「橡皮鸭调试」「技术债务」「第一性原理」等条目。

测试 3（不应命中，纯 AI 发挥）：
```
帮我给我的猫起个名字，它是一只橘猫，很胖，很懒，喜欢趴在键盘上。
```
预期：知识库可能没有直接命中，Agent 应自由发挥推荐。

**Step 4: Commit 测试结果（可选）**

如果测试发现问题，修复后 commit。

---

### Task 6: 创建用户扩展示例文件

**Files:**
- Create: `.prompt-alchemy/knowledge-base/README.md`

**Step 1: 创建 README 说明用户如何扩展**

```markdown
# 用户自定义知识库

在这里创建同名 JSON 文件（people.json / theories.json / concepts.json）即可扩展提示词炼金术的激活钥匙。

格式与内置知识库一致：

{
  "version": 1,
  "entries": [
    {
      "id": "my-custom-entry",
      "name": "自定义概念名",
      "name_en": "English Name",
      "core_idea": "一句话核心说明",
      "scenarios": ["写作", "分析"],
      "keywords": ["关键词1", "关键词2"],
      "prompt_template": "用{concept}来{action}{topic}",
      "popularity": "medium",
      "related": []
    }
  ]
}

规则：
- id 与内置知识库重复 → 你的版本覆盖内置版本
- scenarios 必须从这 7 个中选：写作、分析、策略、创意、编程、沟通、学习
- 文件不存在不影响 skill 运行（纯用内置知识库）
```

**Step 2: Commit**

```bash
git add .prompt-alchemy/
git commit -m "docs: add user knowledge base extension guide"
```

---

## 执行顺序依赖

```
Task 1 (people.json)  ─┐
Task 2 (theories.json) ─┼─→ Task 5 (测试) → Task 6 (扩展示例)
Task 3 (concepts.json) ─┘
Task 4 (SKILL.md)     ─────→ Task 5 (测试)
```

Task 1-4 可并行执行，Task 5 依赖全部完成，Task 6 依赖 Task 5 通过。
