# 用户自定义知识库

在这里创建同名 JSON 文件即可扩展提示词炼金术的激活钥匙。

支持的文件：
- `people.json` — 人物类
- `theories.json` — 理论/模型类
- `concepts.json` — 概念/隐喻类

## 格式

```json
{
  "version": 1,
  "entries": [
    {
      "id": "my-custom-entry",
      "name": "自定义概念名",
      "name_en": "English Name",
      "core_idea": "一句话核心说明（不超过 30 字）",
      "scenarios": ["写作", "分析"],
      "keywords": ["关键词1", "关键词2", "关键词3", "关键词4", "关键词5"],
      "prompt_template": "用{concept}来{action}{topic}",
      "popularity": "medium",
      "related": []
    }
  ]
}
```

## 规则

- **id 重复** → 你的版本覆盖内置版本（可以修正不满意的内容）
- **scenarios** 必须从这 7 个中选：写作、分析、策略、创意、编程、沟通、学习
- **keywords** 至少 5 个，写用户真实会说的词（不要用学术词汇）
- **prompt_template** 必须包含 `{topic}` 等占位符
- **文件不存在不影响 skill 运行**（纯用内置知识库）

## 示例

假设你是医疗行业用户，想加入医学相关概念：

```json
{
  "version": 1,
  "entries": [
    {
      "id": "differential-diagnosis",
      "name": "鉴别诊断思维",
      "name_en": "Differential Diagnosis",
      "core_idea": "系统排除法，从症状推导最可能的病因",
      "scenarios": ["分析", "学习"],
      "keywords": ["诊断", "排查", "鉴别", "排除法", "病因分析", "症状分析"],
      "prompt_template": "用鉴别诊断的思维框架分析{topic}，列出可能的原因并逐一排查",
      "popularity": "medium",
      "related": ["first-principles", "occams-razor"]
    }
  ]
}
```
