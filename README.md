# 🌉 DocuBridge

> **Seamless Korean-to-English Document Translator**
>
> Translate `.docx` files while preserving the original layout and formatting.

![DocuBridge](https://github.com/user-attachments/assets/a0bd5eee-2128-4c76-b6d0-ba10ba9bc8ef)

## 🚀 Key Features

* **Format Preservation:** Maintains tables, fonts, colors, and styles of the original Word document.
* **Easy to Use:** Simply Drag & Drop multiple files to translate them sequentially.
* **Smart Engine Balancing:** Automatically distributes traffic across multiple translation engines (Google, Bing, Alibaba) to prevent IP blocking.
<!--* **Phoenix Mode (Auto-Recovery):** If a translation fails, the system automatically isolates the failed parts and retries them using aggressive recovery logic.
* **Dark Mode:** Supports a sleek dark theme for eye comfort.-->

## 📥 Download

No Python installation required. Just download the executable file.

👉 **[Download Latest Version (v1.0.0)](https://github.com/hanjae98/DocuBridge/releases/latest)**

1. Click the link above and expand **Assets**.
2. Download `DocuBridge.exe`.
3. Run it! (Supports Windows 10/11)

## 📖 How to Use

1. **Select Files:** Click `Select .docx Files` and choose your documents. (Multiple files supported)
2. **Start:** Click `Start Translation`.
3. **Wait & Done:** The progress bar will show the status. The translated file will be saved as `Original_Translated_Timestamp.docx` in the same folder.

<p align="center">

  <img src="https://github.com/user-attachments/assets/e8de138e-3ed7-4e30-a527-84ab823c4417" width="42%">

  <br>

  <em>(▲ Real-time progress tracking with clickable logs)</em>

</p>

---

## 🧠 Technical Architecture

<details>
<summary><strong>Click to expand Implementation Details (System Logic)</strong></summary>

### 1. Multi-Engine Load Balancing
To bypass API rate limits and ensure high availability, **DocuBridge** distributes traffic across multiple translation engines (Google, Bing, Alibaba).
- **Round-Robin Strategy:** Requests are distributed sequentially ($i \pmod N$) to balance load.
- **Dynamic Failover:** Automatically detects engine failures and temporarily excludes them from the active pool to maintain service continuity.

### 2. Concurrency & Idempotency Control
Handles complex Word document structures (e.g., merged cells) where multiple elements may reference the same XML object.
- **Thread-Safe Locking:** Uses `threading.Lock` and unique object memory addresses to prevent race conditions.
- **Stateful Deduplication:** Tracks paragraph states (`READY` → `PROCESSING` → `DONE`) to ensure each segment is translated exactly once.

### 3. FSM-based Task Lifecycle
A Finite State Machine (FSM) manages the lifecycle of every translation unit to prevent data loss.
- **Lifecycle Tracking:** `READY` → `IN_PROGRESS` → `SUCCESS` / `FAILED` / `SKIPPED`.
- **Fault Tolerance:** Robust audit logic ensures no "zombie tasks" remain in the event of unexpected network interruptions.

### 4. Dual-Phase Recovery Strategy
A specialized two-phase approach to maximize the success rate of large-scale documents.
- **Phase 1 (Standard):** Executes translation using the default load-balancing logic.
- **Phase 2 (Recovery):** Identifies failed tasks and performs an **Aggressive Fetch**—issuing simultaneous requests across all available engines to guarantee data retrieval.

### 5. Rule-Based Preprocessing
Optimizes API consumption and preserves document integrity through smart filtering.
- **Format Preservation:** Uses regex to identify and protect numbering (1., A., etc.) and special symbols.
- **Smart Skipping:** Automatically bypasses empty segments, non-target languages, or previously translated content to reduce costs.

</details>

---

## 🛠️ For Developers

You can simply download .exe file via this link: (https://github.com/hanjae98/DocuBridge/releases/latest)
If you want to run the source code directly or contribute:

<details>
<summary><strong>Click to expand How to Run the code</strong></summary>


### Requirements
* Python 3.9+
* Windows OS

### Installation

```bash
# 1. Clone the repository
git clone https://github.com/hanjae98/DocuBridge.git
cd DocuBridge

# 2. Install dependencies (Virtual Environment recommended)
python -m venv venv
.\venv\Scripts\activate
pip install -r requirements.txt

# 3. Run the application
python DocuBridge.py
```

</details>

## 🐞 Bug Report & Contact

If you find any bugs or have suggestions, please open an issue in the **[Issues](https://github.com/hanjae98/DocuBridge/issues)** tab.

* **Email:** [jasonhan9806@gmail.com](mailto:jasonhan9806@gmail.com)
* **Developer:** Han Jaesung (hanjae98)

## 📄 License

This project is licensed under the **MIT License** - see the [LICENSE](LICENSE) file for details.