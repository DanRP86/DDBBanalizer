[README_QuickStart.md](https://github.com/user-attachments/files/22332342/README_QuickStart.md)
# Universal CSV/Excel Analyzer — Quick Start (V0.5.4)

This tool highlights data quality issues in CSV/Excel and prepares optional email drafts to request fixes.

## 1) Install (one-time)

```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
# source .venv/bin/activate

pip install -r requirements.txt
# Optional for old .xls files:
# pip install xlrd==1.2.0
```

> **Note (Windows only):** To save Outlook `.msg` drafts you need `pywin32` (included in requirements) and Outlook installed. Otherwise, the tool saves `.html` drafts.

## 2) Run — INITIAL (discovery)

```bash
python csv_universal_analyzer_en_v0_5_4.py -i your_data.xlsx --debug
```
- Creates **`initial_report_*.xlsx`** (highlights potential outliers & issues) and **`config_your_data.xlsx`**.
- Open `config_*` to adjust rules.

## 3) Configure

In **sheet `CONFIG`**:
- **Row `Main ID`** → `domain=<column>` to set the identifier.
- **Row `Emails`** → `active=YES`, `domain=<group column>`, and optional `email_recipients_column=<column with emails>`.
- **Per column**:
  - `priority` (LOW/MEDIUM/HIGH/VERY_HIGH). **In FINAL, all issues & missing adopt this severity.**
  - `type` (auto-suggested), and any explicit **`min/max`**, `domain` (comma separated) or `regex` constraints.
  - `include_null_in_email=YES` to list missing fields in emails.
  - `email_include_issues=YES` to list issue columns in emails.

## 4) Run — FINAL (apply only your rules)

```bash
python csv_universal_analyzer_en_v0_5_4.py -i your_data.xlsx --debug
```
- Creates **`final_report_*.xlsx`** using **only** your rules.
- Email drafts: one per group (never sent automatically).
  - If there is **no missing** in the shown rows, the **"Missing Fields"** column is hidden.

## Tips
- Keep configs versioned with dates.
- Start simple: define only a few key rules, run FINAL, iterate.

## Troubleshooting
- **Cannot read file**: try `--encoding` or `--sep pipe|semicolon|tab|comma`.
- **No `.msg` files**: you’re not on Windows/Outlook; check the `.html` drafts instead.
- **.xls reading**: install `xlrd==1.2.0`.
