---
name: url-size-checker
description: "This skill should be used when the user wants to check file sizes of URLs without downloading them. It handles reading URLs from a TXT file, concurrently fetching file sizes via HTTP HEAD/GET requests, and generating a formatted Excel report with the results. Trigger phrases include check URL file sizes, get file sizes from URLs, URL size check, URL 文件大小, 检测文件大小, or when the user provides a .txt file containing URLs and wants to know the file sizes."
---

# URL File Size Checker

Check file sizes of URLs listed in a TXT file without downloading them, and generate a formatted Excel report.

## When to Use

- The user provides a `.txt` file containing URLs (one per line) and wants to know the file size of each URL.
- The user asks to "check URL file sizes", "get file sizes", "检测文件大小", or similar requests involving URL size detection.

## Prerequisites

Before running the script, ensure the required Python packages are installed:

```bash
pip install requests openpyxl
```

## Workflow

### Step 1: Identify the Input File

Confirm the user has provided a `.txt` file path. The file should contain one URL per line, each starting with `http://` or `https://`.

### Step 2: Determine the Output Path

If the user specifies an output path, use it. Otherwise, default to placing `urlsizecheckresult.xlsx` in the same directory as the input file.

### Step 3: Install Dependencies

Run the following to ensure dependencies are available:

```bash
pip install requests openpyxl
```

### Step 4: Execute the Script

Run the bundled script located at `scripts/url_size_check.py`:

```bash
python {SKILL_DIR}/scripts/url_size_check.py <input.txt> [output.xlsx]
```

**Arguments:**
- `<input.txt>` — (required) Path to the TXT file containing URLs.
- `[output.xlsx]` — (optional) Path for the output Excel file. Defaults to `urlsizecheckresult.xlsx` in the current directory.

**Example:**

```bash
python {SKILL_DIR}/scripts/url_size_check.py /path/to/urls.txt /path/to/result.xlsx
```

### Step 5: Report Results

After execution, inform the user of:
1. The total number of URLs processed.
2. How many succeeded and how many failed.
3. The total file size (formatted).
4. The path to the generated Excel file.

## Script Details

The script (`scripts/url_size_check.py`):

- Reads URLs from a `.txt` file (one URL per line, must start with `http://` or `https://`).
- Uses 20 concurrent threads with connection pooling to check file sizes efficiently.
- For each URL, first sends an HTTP HEAD request to get `Content-Length`; if unavailable, falls back to a streaming GET request.
- Results are sorted by file size (largest first), with failed URLs at the end.
- Outputs a formatted Excel file with:
  - A summary row showing total size and success/failure counts.
  - Columns: 序号 (Index), URL, 文件大小 (File Size), 状态 (Status).
  - Styled headers, alternating row colors, and proper column widths.

## Error Handling

- If the input file does not exist, the script exits with an error message.
- If no valid URLs are found, the script reports the issue and exits.
- Network errors (timeout, connection refused, etc.) are captured per-URL and reflected in the status column of the Excel output.
