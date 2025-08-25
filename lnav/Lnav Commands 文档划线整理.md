---
date: 2025-08-17
source: https://docs.lnav.org/en/latest/commands.html
---

`lnav` provides a set of advanced commands to manage and analyze log files efficiently, including features like **highlighting**, **previewing command effects**, and **anonymization**. Below is a structured explanation along with example use cases.

---

## 🔍 Command Highlighting and Preview

### ✔️ Preview Behavior

- You can activate the command prompt by pressing `:` (colon).
- As you type, **auto-completion** is supported—press `TAB` to see available commands and parameters.
- Most commands support **interactive preview** of their effect **before execution**:
  - This preview appears shortly after you finish typing, before pressing `Enter`.

#### 🕶 Example — `:open` command
When you start typing `:open somefile.log`, `lnav` will show a **preview of the content of the file**, displaying the **first few lines** to help confirm correctness.

> **Screenshot available:** *open-preview.png*

![[Pasted image 20250818101042.png]]

---

### 🎯 Real-time Preview of Log Filtering with `:filter-out`

The `:filter-out 'pattern'` command allows filtering out log lines based on regular expressions.

- As you type, for instance:
  ```lnav
  :filter-out launchd
  ```
  Any match to "launchd" in the log lines appears in **red** to help verify the filter's scope **without applying it yet**.

> **Screenshot available**: *filter-out-preview.png*
![[Pasted image 20250818101057.png]]
This preview helps ensure you are filtering the correct lines.

---

### ⚠️ Handling Command Errors

If an invalid command or file is referenced (e.g., file doesn’t exist), `lnav` displays an **error in the status bar above the prompt**, such as:

```
:error: Failed to open file '/invalid-path/file.txt'
```

> **Screenshot available**: *open-error.png*
![[Pasted image 20250818101113.png]]
---

## 🔐 Anonymization of Sensitive Data (Optional)

`lnav` has built-in support for sanitizing sensitive information during data export or writing using the `--anonymize` flag on the `:write-*` commands.

### ✅ What Gets Anonymized?

- **IPv4/IPv6 Addresses:** Replaced with generic dummy IP ranges.
- **Usernames in URLs/Emails:** Mapped to random animal names.
- **Passwords:** Replaced with a hash representation.
- **URLs, Paths, UUIDs, Emails**: Processed recursively with replacements or hashing.
- **Credit Card Numbers, Hex Dumps, MAC Addresses, Query Strings**: Also handled using deterministic transformations.

### 📝 Example — Using Anonymization During Export

You can write logs to a file, removing private data during the export:

```lnav
:write-raw-to --anonymize /tmp/anonymized_logs.txt
```

Or, to a CSV file:

```lnav
:write-csv-to --anonymize /tmp/anonymized_table.csv
```

This ensures logs shared for debugging or analysis don’t leak sensitive info.

---

## ✨ Useful Commands Reference (Highlighted)

| Command | Use Case | Description |
|--------|----------|-------------|
| `:filter-out pattern` | Filters out matching lines | Highlights pattern matches in red and allows preview |
| `:open filename` | Preview file content | Shows first few lines without leaving prompt |
| `:annotate` | Attach annotations to a message | Especially useful for adding meta-information |
| `:comment 'message'` | Add inline comments to a specific line | Displays below the marked line |
| `:pipe-to shell-cmd` | Pipe marked lines to shell utilities | Useful for integration, e.g., `| grep` |
| `:redirect-to path` | Redirect command output to a file | For off-screen logging |
| `:write-raw-to [–anonymize] path` | Export unformatted log lines | With optional anonymization |

---

### 📌 Example - Adding a Comment

```lnav
:comment This is where it all went wrong
```
This adds a comment beneath the focused log line.

### 📌 Example - Applying Anonymized Output

```lnav
:write-csv-to --anonymize /tmp/safe_log_data.csv
```
Exports the log lines into a CSV file while scrubbing identifiable fields.

---

## 📘 Key Tips

1. **Use `TAB` Autocomplete:** For commands and even log keywords in filtering.
   - Try double-tapping `TAB` while typing a command.
2. **Error Handling:** Errors appear instantly and are shown in the status line above the command area.
3. **Secure Mode:** Set `LNAVSECURE=1` to disable external commands like `:sh`, `:pipe-to`, and `:open` in production or restricted environments.

#### ⚡ Disable Dangerous Commands via Environment

```bash
LNAVSECURE=1 lnav /path/to/logs/
```

This restricts I/O and shell access to **avoid privilege escalation** risks.

---

## 🗂 Anonymization Summary

| Data Type | Anonymization |
|----------|----------------|
| IPv4 | `10.0.0.0/8` range |
| IPv6 | `2001:db8::/32` |
| URL Usernames / Emails | Random animal names |
| UUID / Hashes | Hashed inputs |
| MAC Addresses | Within range `00:00:5E:00:53:00` |
| Credit Card Numbers | Replaced with 16-digit hashes |
| Paths, URLs, Query Strings | Recursively replaced or scrubbed |

---

## 📋 Other Useful Commands

| Command | Description | Example |
|--------|-------------|---------|
| `:mark` | Toggle a bookmark on the current line | Mark a specific issue line |
| `:hide-unmarked-lines` | Exclude unmarked lines from view | Focus only known issues |
| `:goto 2017-01-01` | Jump to a timestamped line | Quickly scroll to a given date |
| `:highlight 'error|warning'` | Color specific log fragments | Highlights keywords like `error` or `warning` |

---

## 🧪 Try it Yourself

**Sample Workflow**:
1. Open a log file using `:open /path/to/app.log`
2. Type `:filter-out warning` and see the lines preview in red.
3. Confirm and press `Enter` to apply filter.
4. Export safely:
   ```lnav
   :write-raw-to --anonymize /tmp/filtered.txt
   ```

> Tip: Combine `:annotate`, `:goto`, and `:comment` to create a well-documented troubleshooting journal.

---

### 🧹 Clearing and Disabling Actions

- `:clear-filter-expr` – remove filters
- `:disable-filter 'pattern'` – temporary disable specific filters
- `:delete-filter 'pattern'` – delete a filter completely
- `:disable-word-wrap` / `:enable-word-wrap` – toggle word wrapping

---

## 📝 Summary

- `lnav` enables **in-line filtering**, **previews**, and real-time visual feedback.
- Use **anonymization** during exports to safely share data.
- **Tab completion**, **commenting**, and **filtering** allow for effective log browsing and review.

This guide serves as a compact, practical notebook for working with `lnav` efficiently and securely.


