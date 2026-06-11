```markdown
# 🚀 Kelasku Info Automated UI Test & Reporter

This project is an automated testing script built with Python and **Playwright**. It performs an end-to-end UI test that logs into a web application, navigates to the "Kelasku" (My Class) module, and attempts to create a new informational post. 

It is designed to be resilient—if it cannot find the "Add" button, it will automatically attempt to clear existing posts first. Finally, it generates a comprehensive PDF report with visual evidence and emails it to the team.

---

## ✨ Features

* **Automated Browser Interaction:** Asynchronously navigates pages, handles loading spinners, types into iframes (Rich Text Areas), and clicks buttons using Playwright.
* **Smart Fallback Logic:** If the "Create Info" button is missing, the script automatically searches for and deletes existing entries to make room for the new test.
* **Dynamic PDF Reporting:** Uses `fpdf` and `matplotlib` to compile a professional PDF containing:
  * A pass/fail summary donut chart.
  * A detailed table of step-by-step test logs.
  * A screenshot of the browser at the end of the test.
* **Automated Email Dispatch:** Integrates with the Mailjet API to automatically send the generated PDF report as an email attachment.

---

## 📋 Prerequisites

Before running the script, ensure you have **Python 3.8+** installed. You will also need to install the required third-party dependencies.

### 1. Install Python Packages
Run the following command in your terminal:

```bash
pip install playwright matplotlib fpdf2 mailjet_rest

```

### 2. Install Playwright Browsers

Playwright requires browser binaries to run. Install the Chromium browser by running:

```bash
playwright install chromium

```

---

## ⚙️ Configuration

This script relies on environment variables for dynamic configuration and expects a `settings.py` file to be present in your project directory.

### Environment Variables

You can customize the test behavior by setting the following environment variables:

| Variable Name | Description | Default Value |
| --- | --- | --- |
| `KELASKU_INFO_TEXT` | The text to be typed into the Rich Text Area during the test. | `Test 2` |
| `WAIT_AFTER_ACTION_SECONDS` | Seconds to wait before taking the final screenshot. | `3` |
| `SCREENSHOT_PATH` | The file name/path for the saved screenshot. | `screenshot2_kelasku_info.png` |
| `REPORT_PATH` | The file name/path for the generated PDF report. | `test_report_kelasku_info.pdf` |

### Required `settings.py` File

The script imports `EmailConfig` and `LoginConfig` from a local `settings.py` file. Ensure you have this file set up in the same directory. Here is an example of what it should look like:

```python
# settings.py
from dataclasses import dataclass
import os

@dataclass
class LoginConfig:
    login_url: str = "[https://your-website.com/login](https://your-website.com/login)"
    username: str = "your_email@example.com"
    password: str = "your_password"
    timeout_ms: int = 15000
    headless: bool = True

@dataclass
class EmailConfig:
    enabled: bool = os.getenv("SEND_EMAIL", "false").lower() == "true"
    api_key: str = os.getenv("MAILJET_API_KEY", "")
    api_secret: str = os.getenv("MAILJET_API_SECRET", "")
    sender_email: str = "sender@example.com"
    sender_name: str = "Automated QA"
    recipient_email: str = "team@example.com"
    recipient_name: str = "Dev Team"
    subject: str = "Kelasku Info Test Report"
    body: str = "Please find the attached automated test report."

```

---

## 🚀 Usage

Once your dependencies are installed and your `settings.py` is configured, you can run the test script simply by executing:

```bash
python main.py

```

*(Assuming your script file is named `main.py`)*

### Test Execution Flow:

1. Launches a headless Chromium browser.
2. Logs into the application.
3. Waits for the loading spinner to disappear.
4. Navigates to the **Kelasku** menu.
5. Attempts to click the "Add" icon. *(If not found, it deletes an existing entry and tries again).*
6. Fills out the rich text iframe and clicks "Buat Info" (Create Info).
7. Takes a final screenshot.
8. Generates the PDF report and sends the email.

---

## 📂 Output Files

After the test concludes, you will find two newly generated files in your project directory:

* **`screenshot2_kelasku_info.png`**: The visual evidence of the page after the test completes.
* **`test_report_kelasku_info.pdf`**: The detailed PDF report containing the logs, success rate chart, and the screenshot.
