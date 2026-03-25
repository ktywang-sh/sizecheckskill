# URL Size Checker Skill

一个 CodeBuddy Skill，用于批量检测 URL 对应文件的大小（无需下载），并生成带格式的 Excel 报告。

## 功能特点

- 📄 从 `.txt` 文件读取 URL 列表（每行一个）
- ⚡ 20 线程并发检测，高效快速
- 🔍 先尝试 HTTP HEAD 获取 `Content-Length`，不可用时自动回退到 GET 请求
- 📊 结果按文件大小降序排列，失败项排在最后
- 📥 输出带格式的 Excel 报告（汇总行 + 蓝色表头 + 斑马纹）

## 安装

### 作为 CodeBuddy Skill 使用

1. 下载 `url-size-checker.zip`
2. 在 CodeBuddy 中导入 Skill
3. 当你提供一个包含 URL 的 `.txt` 文件时，Skill 会自动触发

### 独立命令行使用

```bash
# 安装依赖
pip install requests openpyxl

# 运行
python scripts/url_size_check.py <input.txt> [output.xlsx]
```

## 使用方法

### 1. 准备输入文件

创建一个 `.txt` 文件，每行一个 URL：

```text
https://example.com/file1.zip
https://example.com/file2.pdf
https://example.com/file3.mp4
```

> URL 必须以 `http://` 或 `https://` 开头，其余行会被自动忽略。

### 2. 运行检测

```bash
python scripts/url_size_check.py urls.txt result.xlsx
```

**参数说明：**

| 参数 | 是否必填 | 说明 |
|------|---------|------|
| `input.txt` | 必填 | 包含 URL 的文本文件 |
| `output.xlsx` | 可选 | 输出路径，默认 `urlsizecheckresult.xlsx` |

### 3. 查看结果

运行完成后终端会输出汇总信息：

```
📄 读取文件: /path/to/urls.txt
🔗 发现 100 个有效 URL，开始检测...
  进度: 100/100 (100%)

📊 检测完成:
   总数: 100
   成功: 95
   失败: 5
   总大小: 1.23 GB

✅ 结果已保存: /path/to/result.xlsx
```

### 4. Excel 报告内容

| 列 | 说明 |
|----|------|
| 序号 | 行号 |
| URL | 原始链接 |
| 文件大小 | 格式化显示（B / KB / MB / GB） |
| 状态 | ✓ 成功 / ✗ 超时 / ✗ 连接失败 等 |

首行为汇总信息：文件总大小、成功/失败数量。

## 项目结构

```
url-size-checker/
├── SKILL.md                      # Skill 定义文件
├── README.md                     # 本文件
└── scripts/
    └── url_size_check.py         # 核心检测脚本
```

## 依赖

- Python 3.10+
- [requests](https://pypi.org/project/requests/) >= 2.28.0
- [openpyxl](https://pypi.org/project/openpyxl/) >= 3.1.0

## License

MIT
