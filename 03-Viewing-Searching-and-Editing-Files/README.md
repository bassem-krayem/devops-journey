# What We Learned

### The `file` Command

In Linux, we don't rely solely on file extensions (like `.txt` or `.jpg`) to determine a file's type, as extensions can be misleading or missing. Instead, we use the `file` command to look at the actual data within the file.

**Usage:**
`file <path_to_file>`

It analyzes the file's header and tells you exactly what it is (e.g., an ASCII text file, a JPEG image, or an ELF executable).

### The `grep` Command

`grep` (Global Regular Expression Print) is used to search for specific text or regex patterns within files. It scans the file and prints every line that contains a match.

**Basic Usage:**
`grep "pattern" <file>`

**Useful Flags:**

- **`-i` (Ignore Case):** Searches for the pattern regardless of capitalization (e.g., "linux" will match "Linux" or "LINUX").
- **`-v` (Invert Match):** Instead of showing matches, it prints every line that **does not** contain the pattern. This is excellent for filtering out noise or unwanted data from logs.

### The `wc` Command

The `wc` (Word Count) command is used to display the number of lines, words, and byte counts in a file. It is a vital tool in DevOps for analyzing log files or script outputs.

**Basic Usage:**
`wc [options] <file>`

**Common Options:**

- **`-l` (Lines):** Counts the total number of lines in the file.
- **`-w` (Words):** Counts the total number of words based on whitespace separation.
- **`-c` (Bytes):** Counts the total number of bytes (which usually corresponds to the total number of characters, including hidden ones).

**Default Behavior:**
If you run `wc` without any options (e.g., `wc file.txt`), it will return all three statistics at once in the following order:
`Lines` | `Words` | `Bytes` | `Filename`

### The `nano` Editor

The `nano` program is a simple, user-friendly, and terminal-based text editor. It is used to write or modify files directly within the command line.

**Opening a File:**
`nano <file_or_path>`

If the file exists, it opens for editing. If it doesn’t exist, `nano` creates a new buffer that will be saved as a file upon exit. You can use the **arrow keys** to navigate through the text.

**The Workflow (Write-Save-Exit):**

1. **Writing:** Simply type your text into the editor.
2. **Saving (Write Out):** Press `Ctrl + O`. You will see the filename at the bottom; press `Enter` to confirm. You can continue editing after saving.
3. **Exiting:** Press `Ctrl + X`.

**Note:** If you have unsaved changes when pressing `Ctrl + X`, `nano` will ask if you want to save them. Press `Y` for Yes or `N` for No, then `Enter` to confirm the filename and exit.

### The `vim` Editor

Vim is a highly efficient and powerful modal text editor. Unlike `nano`, which is always in editing mode, `vim` operates using different "modes" for navigation, commands, and text entry.

**Opening a File:**
`vim <file_or_path>`

**The Two Primary Modes:**

1. **Command Mode (Default):** This is the mode you start in. Here, the keyboard is used for navigation and shortcuts (like copying, deleting, or searching). **Be careful:** typing randomly in this mode can trigger unexpected commands.
2. **Insert Mode:** This is where you actually type your text.

**The Workflow:**

1. **Navigating:** Use the **arrow keys** while in Command Mode to move through the text.
2. **Entering Insert Mode:** Press `i` (for Insert). You will see `-- INSERT --` at the bottom of the screen. Now you can type normally.
3. **Returning to Command Mode:** When finished typing, press the `Esc` (Escape) key. This "locks" the text and lets you run commands again.

**Saving and Exiting:**
In Command Mode, type a colon (`:`) followed by these letters:

- **`:w`** – **W**rite (Save the file).
- **`:q`** – **Q**uit.
- **`:wq`** – Save and Quit in one step.
- **`:q!`** – Quit without saving (Discard changes).

**Summary:** Open → `i` to write → `Esc` to stop writing → `:wq` to finish.

### Essential Vim Command Mode Operations

After pressing **`Esc`** to exit Insert Mode, you can use these powerful shortcuts to navigate and edit your file quickly.

**1. Basic Navigation**

- **`G`**: Move the cursor to the last line of the document.
- **`gg`**: Move the cursor to the first line of the document.
- **`0`** (zero): Move to the beginning of the current line.
- **`$`**: Move to the end of the current line.

**2. Editing & Deleting**

- **`x`**: Delete the single character under the cursor.
- **`dd`**: Delete (cut) the entire current line.
- **`u`**: **Undo** the last action (the "life-saver" command).
- **`Ctrl + r`**: **Redo** the action you just undid.

**3. Copy & Paste**

- **`yy`**: "Yank" (copy) the current line.
- **`p`**: Paste the copied/cut text after the cursor.

**4. Searching**

- **`/pattern`**: Type `/` followed by a word to search forward in the file. Press **`n`** to jump to the next match.

**Pro-Tip for DevOps:**
If you need to edit a configuration file but forgot to use `sudo`, you don't have to quit and restart. In Command Mode, you can try to force a save (if you have permissions) using `:w!`.
