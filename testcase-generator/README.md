# 测试用例生成器 (testcase-generator)

一个用于从软件需求文档中精准提炼测试点、设计结构化测试用例的 Claude Code Skill。

## 能做什么

- 把需求文档（PRD / 设计文档 / 接口文档 / 原型截图）转化为结构化测试点
- 系统性覆盖**正常 / 边界 / 异常**三类场景
- 处理 PDF / Word / Markdown / 图片等格式，识别流程图、UI、状态图、表格中的测试信息
- 输出**禅道(ZenTao)** 标准导入格式（Excel / CSV），或 XMind 思维导图

## 何时触发

当你说：生成测试用例、设计测试点、测试覆盖、写测试、从需求文档生成禅道用例 等。

## 用法

```
/testcase-generator 请为这个需求文档生成测试用例：<粘贴需求或上传文件>
```

或直接在对话中描述需求并要求"生成测试用例 / 导出禅道 Excel / 导出 XMind"。

## 目录结构

```
testcase-generator/
├── SKILL.md                      # 主入口：核心原则、三要素模板、工作流
├── references/
│   ├── reading-requirements.md   # 如何阅读需求文档 + 图文混合处理
│   ├── boundary-analysis.md      # 边界约束：等价类/边界值/判定表/状态转换/异常清单
│   └── output-format.md          # 禅道字段、Markdown 中间稿、Excel/CSV、XMind 格式
└── scripts/
    ├── generate_excel.py         # Markdown 表格 → 禅道 CSV/Excel
    └── analyze_document.py       # PDF/Word/图片/文本 → 分析报告
```

## 脚本用法

```bash
# 需求文档分析（提取文字与图片列表）
python scripts/analyze_document.py requirement.pdf -o ./extracted -r ./analysis_report.md

# 测试用例 Markdown 表格 → 禅道 CSV（仅需标准库）
python scripts/generate_excel.py testcases.md testcases.csv

# → Excel（需 openpyxl: pip install openpyxl）
python scripts/generate_excel.py testcases.md testcases.xlsx
```

### 可选依赖

| 功能 | 依赖 | 缺失时 |
|-----|------|-------|
| 生成 .xlsx | `openpyxl` | 提示改用 .csv |
| 解析 PDF | `pdfplumber` | 给出安装提示 |
| 解析 Word | `python-docx` | 给出安装提示 |
| 图片 OCR | `pytesseract` + `Pillow` + 本机 Tesseract | 给出安装提示 |

`.md/.txt` 解析与 `.csv` 生成仅用 Python 标准库，无需额外安装。

## 测试点设计三要素

```
[操作动作 + 条件] 场景下，[验证对象] 是否 [预期方向]
预期结果：[具体的、可验证的结果描述]
```

## 输出字段（禅道）

所属模块 | 用例标题 | 前置条件 | 操作步骤 | 预期结果 | 优先级(P0–P3) | 用例类型 | 标签
