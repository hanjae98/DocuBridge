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

![Process Screenshot](https://github.com/hanjae98/DocuBridge/assets/PLACE_YOUR_PROCESS_IMAGE_LINK_HERE.png)
*(▲ Real-time progress tracking with clickable logs)*

---

## 🧠 Technical Architecture

<details>
<summary><strong>Click to expand Implementation Details (System Logic)</strong></summary>

### 1. Multi-Engine Load Balancing (Round-Robin)
To minimize API rate limiting, **DocuBridge** utilizes a **Round-Robin** strategy across heterogeneous translation engines (Google, Bing, Alibaba).
- **Traffic Distribution:** Requests are distributed sequentially ($i \pmod N$).
- **Failover:** If an engine fails, it is temporarily excluded from the active pool.

### 2. Concurrency Control & Idempotency
Word documents often contain merged cells that share the same XML object ID.
- **Object ID Locking:** Uses `threading.Lock` and Python's `id()` to identify unique paragraph objects.
- **State Management:** Prevents duplicate translations (Race Conditions) by tracking the state of each paragraph (`UNTOUCHED` → `QUEUED` → `PROCESSING` → `DONE`).

### 3. Finite State Machine (FSM) Lifecycle
Ensures data integrity by tracking the full lifecycle of every translation task.
- **States:** `READY` → `IN_PROGRESS` → `SUCCESS` / `FAILED` / `SKIPPED`.
- **Audit:** Ensures no tasks are lost ("Zombie processes") during network interruptions.

### 4. Phoenix Protocol (Two-Phase Commit)
Guarantees maximum success rate through a two-phase strategy.
- **Phase 1 (Normal):** Standard load balancing translation.
- **Phase 2 (Recovery):** Filters only failed tasks and executes an **Aggressive Fetch** (simultaneous requests to all engines) to recover missing data.

### 5. Heuristic Preprocessing
- **Regex Filtering:** Automatically detects and preserves numbering formats (e.g., 1. 2. or A. B.).
- **Smart Skipping:** Skips content that is already translated or contains no Korean characters to save resources.

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
git clone [https://github.com/hanjae98/DocuBridge.git](https://github.com/hanjae98/DocuBridge.git)
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