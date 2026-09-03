# PowerShell Fast Duplicate File Cleaner

A high-performance, interactive PowerShell script that scans the current directory for exact duplicate files using a two-pass size pre-filter and MD5 hashing. It intelligently ranks files chronologically, defaults to preserving the newest version, and gives you complete interactive control over file retention.

[![PowerShell](https://img.shields.io/badge/PowerShell-5.1%2B%20%7C%207.x-blue.svg)](https://github.com/PowerShell/PowerShell)
[![Platform](https://img.shields.io/badge/platform-Windows%20%7C%20macOS%20%7C%20Linux-lightgray.svg)](https://github.com/PowerShell/PowerShell)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

---

## 📌 Features

- **⚡ Two-Pass Performance Optimization**
  Pre-filters files by byte size before computing hashes. Unique-sized files are skipped immediately, dramatically speeding up execution on large directories.
- **🔒 High Integrity with MD5**
  Computes MD5 hashes only on size-colliding candidates, guaranteeing exact content matching without false positives.
- **🕒 Smart Chronological Ranking**
  Automatically sorts duplicate groups by `LastWriteTime` (newest first).
- **⌨️ One-Key Default Action**
  Pressing **Enter** automatically selects option `[1]` (the newest file), allowing rapid processing of multiple duplicate groups.
- **🔍 Verbose Real-Time Progress**
  Displays live step-by-step progress, including total file count, size-group breakdowns, live hashing indicators, and explicit deletion feedback.
- **🛡️ Safe Path Handling**
  Utilizes PowerShell's `-LiteralPath` parameter to safely manipulate filenames containing special wildcard characters (e.g., `[`, `]`, `$`, `#`).
- **⏭️ Non-Destructive Skip Option**
  Option `[0]` allows skipping any group without modifying or deleting files.

---

## 📋 Requirements

- **PowerShell**: Version 5.1 or higher (Compatible with PowerShell Core 7.x on Windows, macOS, and Linux).
- **Permissions**: Read and Delete permissions in the execution folder.

---

## 🚀 Quick Start

### 1. Download / Clone
Clone the repository or save `Find-Duplicates.ps1` to your desired directory:

```bash
git clone https://github.com/your-username/powershell-duplicate-cleaner.git
cd powershell-duplicate-cleaner
```

### 2. Execution Policy
If PowerShell blocks script execution on Windows, temporarily unlock execution for your current session:

```powershell
Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope Process
```

### 3. Run the Script
Navigate to the directory you wish to clean and execute the script:

```powershell
.\Find-Duplicates.ps1
```

---

## 🖥️ Example Terminal Output

```text
Scanning current directory for files...
Found 12 file(s) in total.

Grouping files by size to identify potential duplicates...
Found 2 size group(s) containing 5 candidate files:

  Size Match: 142.5 KB (145920 bytes) (3 files)
    - project_v1.pdf
    - project_copy.pdf
    - project_v2.pdf
  Size Match: 12.1 KB (12390 bytes) (2 files)
    - notes_backup.txt
    - notes.txt

Computing MD5 hashes for candidate files...
[1/5] Hashing MD5: project_v1.pdf (142.5 KB)... [DONE]
[2/5] Hashing MD5: project_copy.pdf (142.5 KB)... [DONE]
[3/5] Hashing MD5: project_v2.pdf (142.5 KB)... [DONE]
[4/5] Hashing MD5: notes_backup.txt (12.1 KB)... [DONE]
[5/5] Hashing MD5: notes.txt (12.1 KB)... [DONE]

Analysis complete! Found 2 group(s) of exact MD5 duplicates.

==================================================
MD5 Hash : E2FC714C4727EE9395F324CD2B750011
Duplicates: 3 files
==================================================
[1] (NEWEST - DEFAULT) project_v2.pdf
    Size: 142.5 KB | Modified: 09/03/2026 14:10:00
[2] project_copy.pdf
    Size: 142.5 KB | Modified: 09/01/2026 09:30:00
[3] project_v1.pdf
    Size: 142.5 KB | Modified: 08/25/2026 11:15:00
[0] Keep ALL files (Skip this group)
--------------------------------------------------
Enter file number to KEEP [Press Enter for 1 (Newest)]: 

Keeping: project_v2.pdf
Deleting: project_copy.pdf... [DELETED]
Deleting: project_v1.pdf... [DELETED]

Duplicate cleanup complete!
```

---

## ⚙️ How It Works

```
┌────────────────────────────────┐
│  1. Scan Current Directory     │
└──────────────┬─────────────────┘
               │
               ▼
┌────────────────────────────────┐
│  2. Group Files by Byte Size   │
└──────────────┬─────────────────┘
               │ (Discard unique sizes)
               ▼
┌────────────────────────────────┐
│  3. Compute MD5 Hashes         │
└──────────────┬─────────────────┘
               │ (Discard non-matching hashes)
               ▼
┌────────────────────────────────┐
│  4. Sort Groups Chronologically│
└──────────────┬─────────────────┘
               │
               ▼
┌────────────────────────────────┐
│  5. Interactive Selection      │
└────────────────────────────────┘
```

1. **Size Filtering**: Scans files and filters out any file with a unique byte length.
2. **Hash Comparison**: Reads stream data only for candidates with size collisions and generates MD5 hashes.
3. **Sorting**: Groups files with matching hashes and orders them from newest to oldest (`LastWriteTime`).
4. **Action**: Prompts the user to retain one file while safely removing unselected duplicates via `Remove-Item -LiteralPath`.

---

## ⚠️ Important Notes & Best Practices

- **Backup Important Data**: Always ensure you have backups before running bulk cleanup tools.
- **Read-Only Files**: System or locked/read-only files will raise a PowerShell exception unless run with elevated permissions.
- **Filename Special Characters**: The script utilizes `-LiteralPath` everywhere to prevent wildcard evaluation issues with filenames containing brackets or special symbols.

---

## 📄 License

This project is open-source and available under the [MIT License](LICENSE).
