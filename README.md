---
AIGC:
  ContentProducer: '001191110102MAD55U9H0F10002'
  ContentPropagator: '001191110102MAD55U9H0F10002'
  Label: '1'
  ProduceID: '91557772-b0cb-44ac-a45b-3abdb392582a'
  PropagateID: '91557772-b0cb-44ac-a45b-3abdb392582a'
  ReservedCode1: '0150dc5a-f241-415d-9bd0-683bc2ecb905'
  ReservedCode2: '0150dc5a-f241-415d-9bd0-683bc2ecb905'
---

# 软考刷题库

纯前端单 HTML 应用，支持多平台刷题、云端同步、本地 Python 自动出题。

## 文件结构
```
index.html              刷题库 Web 应用（双击即可打开）
generate_questions.py   本地出题工具（PDF → OCR → AI出题 → JSON）
pdf_ocr_toolkit.py      OCR 核心模块
requirements.txt        Python 依赖
demo_questions.json     第1章示例题库（34题，可直接导入）
```

## 快速开始

### 1. 刷题
直接双击 `index.html` 打开浏览器即可使用，无需安装任何东西。

首次使用请先在「导入添题」页面导入 `demo_questions.json`（第1章 34 道题），
然后在「题库管理」中审核通过，即可开始刷题。

### 2. 出题（本地 Python）
把新章节的 PDF 放到本目录，运行：
```bash
python -m pip install -r requirements.txt
python generate_questions.py 你的教材.pdf -o 题库.json
```
然后在应用的「导入添题」页面导入生成的 JSON 文件。

### 3. 云同步（可选）
点击应用右上角 ⚙ 设置，填入 Supabase 的 URL 和 Public Key 即可启用。
填写后数据自动双向同步，手机/公司电脑/家里电脑都能刷同一套题库。

### 4. 部署到 GitHub Pages
```bash
git init
git add index.html demo_questions.json
git commit -m "软考刷题库"
git remote add origin https://github.com/你的用户名/ruankao-quiz.git
git push -u origin main
# 然后在 GitHub 仓库 Settings → Pages → 选 main 分支即可
```
部署后任何设备打开 `https://你的用户名.github.io/ruankao-quiz/` 即可刷题。

## 功能一览
| 功能 | 说明 |
|---|---|
| 逐题练习 | 按章节/难度筛选，逐题作答，即时反馈+解析 |
| 模拟考试 | 随机抽题+倒计时，交卷后统一判分+逐题详情 |
| 错题本 | 自动收录做错的题 |
| 题库管理 | 审核/拒绝/编辑/删除/手动添题 |
| 导入添题 | 导入 JSON 题库文件 或 手动粘贴 JSON |
| 导出题库 | 一键导出为 JSON 文件 |
| 云同步 | Supabase 双向同步，多设备共享数据 |

## 数据架构
- **前端存储**：浏览器 localStorage（离线可用）
- **云同步**：Supabase REST API（可选，留空则纯离线）
- **出题工具**：本地 Python，RapidOCR + 规则模板出题
- **数据格式**：JSON，结构为 `{"questions": [...], "exportTime": "..."}`