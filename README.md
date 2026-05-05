# Test Automation - Chat Translator

An automated test suite for web-based applications using Playwright and Python. This tool specifically targets the **Pixels Suite Chat Translator** but can be adapted for similar web applications.

## Overview

This automation framework:
- **Reads test cases** from Excel spreadsheets
- **Automates UI interactions** using Playwright
- **Captures actual outputs** from the application
- **Compares results** against expected values
- **Records test results** back to the Excel file

## Features

✅ Automatic Excel column detection (flexible header matching)  
✅ Support for merged cells in Excel  
✅ Retry logic with configurable wait times  
✅ Overlay/cookie consent dismissal  
✅ Real-time result updates to Excel  
✅ Verbose logging for debugging  
✅ Headless and UI modes  
✅ Custom timeout and delay configurations  

## Requirements

- Python 3.8+
- Playwright
- openpyxl

## Installation

1. **Clone the repository:**
   ```bash
   git clone <repository-url>
   cd test_automation
   ```

2. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Install Playwright browsers:**
   ```bash
   playwright install chromium
   ```

## Excel Format

Your test cases Excel file should have the following structure:

| Singlish Input | Expected Sinhala Output | Actual Output | Status |
|---|---|---|---|
| namaskaaraya | නමස්කාරයා | (auto-filled) | (auto-filled) |
| ayubowan | ආයුබෝවන් | (auto-filled) | (auto-filled) |

### Column Names (Flexible Matching)

The tool automatically detects columns using these defaults:

**Input columns:** `Singlish`, `Input`, `Test Input`, `Source`, `Sentence`, `Text`, etc.  
**Expected output:** `Sinhala`, `Expected Output`, `Expected Sinhala`, etc.  
**Actual output:** `Actual Output`, `Actual` (auto-created if missing)  
**Status:** `Status`, `Result`, `Pass/Fail` (auto-created if missing)  

## Usage

### Basic Usage

```bash
python test_automation.py
```

This will:
- Use the default Excel file: `Assignment 1 - Test cases IT23750906.xlsx`
- Use the sheet named ` Test cases`
- Open the Pixels Suite Chat Translator
- Run tests in headless mode

### Advanced Usage

```bash
python test_automation.py \
  --excel "path/to/your/testcases.xlsx" \
  --sheet "Sheet1" \
  --url "https://yourapp.com" \
  --headless \
  --wait-ms 3000 \
  --type-delay-ms 50
```

## Command-Line Arguments

| Argument | Default | Description |
|---|---|---|
| `--excel` | `Assignment 1 - Test cases IT23750906.xlsx` | Path to Excel test file |
| `--sheet` | ` Test cases` | Name of Excel sheet to use |
| `--header-row` | Auto-detect | Row number containing headers |
| `--input-col` | Auto-detect | Column name for test inputs |
| `--expected-col` | Auto-detect | Column name for expected outputs |
| `--actual-col` | Auto-create | Column name for actual outputs |
| `--status-col` | Auto-create | Column name for test status |
| `--url` | `https://www.pixelssuite.com/chat-translator` | Application URL to test |
| `--output` | Same as `--excel` | Output file path |
| `--wait-ms` | `5000` | Wait time after action (ms) |
| `--retries` | `8` | Number of retry attempts |
| `--retry-wait-ms` | `1000` | Wait between retries (ms) |
| `--type-delay-ms` | `30` | Delay between keystrokes (ms) |
| `--timeout-ms` | `60000` | Max time to wait for elements (ms) |
| `--slow-mo-ms` | `0` | Browser slow motion (ms) |
| `--save-every` | `0` | Save Excel every N rows (0 = only at end) |
| `--headless` | `false` | Run browser in headless mode |
| `--keep-open` | `false` | Keep browser open after tests |

## Examples

### Run with custom Excel file and visible browser:
```bash
python test_automation.py --excel "my_tests.xlsx" --headless false
```

### Save progress every 10 rows:
```bash
python test_automation.py --save-every 10
```

### Test a different application:
```bash
python test_automation.py --url "https://example.com/translator" --wait-ms 2000
```

### Keep browser open for manual inspection:
```bash
python test_automation.py --keep-open
```

## Test Results

The tool generates the following status values in the Excel file:

| Status | Meaning |
|---|---|
| `PASS` | Output matches expected value exactly |
| `FAIL` | Output does not match expected value |
| `COLLECTED` | Output collected but no expected value to compare |
| `UI Error` | Error during UI interaction |

## Troubleshooting

**Issue: "Could not find Chat UI locators"**
- The tool couldn't find the input/output textareas
- Check the URL and ensure the application has loaded
- Try increasing `--timeout-ms`

**Issue: "File not found"**
- Verify the Excel file path is correct
- Use absolute paths or ensure relative paths are from the script directory

**Issue: "Column not detected"**
- Check your Excel headers match expected column names
- Use `--header-row` to specify the header row number manually
- Use `--input-col`, `--expected-col` to explicitly specify columns

**Issue: Output not captured**
- Try increasing `--wait-ms` to allow more time for the application to respond
- Try increasing `--retry-wait-ms` for slower applications

## Environment Variables

You can set the frontend URL via environment variable:

```bash
export FRONTEND_URL="https://yourapp.com"
python test_automation.py
```

## Architecture

- **Playwright** - Browser automation
- **openpyxl** - Excel file handling
- **argparse** - Command-line argument parsing
