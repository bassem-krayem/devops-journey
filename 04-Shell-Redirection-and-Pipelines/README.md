# What We Learned: I/O Redirection

### The Two Output Streams

In the Linux shell, commands communicate through two distinct output streams. Distinguishing between them is crucial for debugging and logging in DevOps.

1.  **Standard Output (stdout):** The stream where the command sends its successful data. Its file descriptor is `1`.
2.  **Standard Error (stderr):** The stream reserved specifically for error messages. Its file descriptor is `2`.

### Redirection (`>` and `>>`)

We can capture the **stdout** of a command and send it to a file instead of the terminal.

- **`>` (Overwrite):** Redirects output to a file. If the file exists, it **deletes** the old content and replaces it with the new output. If the file doesn't exist, it creates it.
  - _Example:_ `ls > files.txt`
- **`>>` (Append):** Redirects output to a file but **adds** it to the end of the existing content without deleting anything.
  - _Example:_ `echo "New log entry" >> logs.txt`

### Redirecting Errors (`2>`)

Standard redirection (`>` or `>>`) only captures `stdout`. If a command fails, the error message will still appear on the screen and won't be saved to your file. To capture these:

- **`2>`:** Redirects the **stderr** stream to a file.
  - _Example:_ `ls non_existent_folder 2> error.log`
- **`2>>`:** Appends the error to the end of a file.
- **`&>`:** A useful shortcut to redirect **both** stdout and stderr to the same file.

**Summary Table:**
| Operator | Redirects | Mode |
| :--- | :--- | :--- |
| `>` | stdout (1) | Overwrite |
| `>>` | stdout (1) | Append |
| `2>` | stderr (2) | Overwrite |
| `2>>` | stderr (2) | Append |
| `&>` | stdout & stderr | Overwrite |

### The Pipe Operator (`|`)

The pipe is one of the most powerful tools in the Linux CLI. it allows you to chain commands together by taking the **Standard Output (stdout)** of the command on the left and feeding it as **Standard Input (stdin)** to the command on the right.

**How it works:**
`command1 | command2 | command3`

Think of it as an assembly line where each command performs a specific task on the data before passing it to the next one.

**Common Use Cases:**

1.  **Filtering Text:**
    `cat data.txt | grep "Error"`
    _(Reads the file and passes the text to grep to find only lines containing "Error".)_

2.  **Counting Results:**
    `ls /etc | wc -l`
    _(Lists all files in /etc and passes that list to wc to count how many files exist.)_

3.  **JSON Processing (DevOps/API Testing):**
    `curl https://api.example.com/data | jq`
    _(Fetches raw, messy JSON data from an API and passes it to `jq` to "pretty-print" and format it so it's readable in the terminal.)_

4.  **Complex Chaining:**
    `cat access.log | grep "404" | wc -l`
    _(Reads a log, finds all 404 errors, and then counts how many there are.)_

**Why it's useful:**
Instead of saving the output of every command to a temporary file, you can process data "on the fly," making your workflow much faster and cleaner.

### The `history` Command

The `history` command is a time-saver that allows you to view, search, and reuse commands you have previously typed. Every command is stored in a list, labeled with a specific index number.

**Basic Usage:**
`history`

**Repeating Commands:**

- **`!!` (Double Bang):** Repeats the exact last command you executed. This is extremely useful if you forgot to type `sudo` (e.g., `sudo !!`).
- **`!n`:** Repeats a specific command by its number from the history list (e.g., `!42`).

**Searching History:**

- **`Ctrl + R` (Reverse Search):** This enters an interactive search mode.
  1. Press `Ctrl + R` and start typing a part of the command.
  2. It will show the most recent match.
  3. Press `Ctrl + R` again to cycle backward through previous matches.
  4. Press **Enter** to execute the command or the **Arrow keys** to edit it before running.

**Pro-Tip:**
You can pipe history into grep to find specific commands you used in the past:
`history | grep "docker"`

### The `sort` and `uniq` Commands

These commands are essential for data processing and cleaning. They allow you to organize messy output and identify unique or recurring patterns in your logs or files.

**Important Note:** The `uniq` command only detects duplicate lines that are **adjacent** (right next to each other). Because of this, you almost always need to use `sort` before `uniq`.

**1. The `sort` Command**
By default, `sort` organizes lines alphabetically (A to Z) or numerically.

- **Basic Usage:** `sort <file>`
- **Useful Flag:** `-n` (Numerical sort) – used when you want to sort numbers correctly (so "10" comes after "2").

**2. The `uniq` Command**
Once the data is sorted, `uniq` filters out the repeated lines.

- **Basic Usage:** `sort file.txt | uniq`
- **The `-c` (Count) Flag:** This is the most powerful way to use `uniq`. It generates a report showing how many times each line appeared in the file.

**Real-World Example:**
If you want to see which IP addresses are hitting your server the most from an access log:
`cat access.log | cut -d' ' -f1 | sort | uniq -c | sort -nr`

**Breakdown of the example:**

1.  `cat` – Reads the log.
2.  `cut` – Grabs only the IP address column.
3.  **`sort`** – Groups identical IPs together.
4.  **`uniq -c`** – Counts the occurrences of each unique IP.
5.  `sort -nr` – Sorts the final count numerically in reverse (highest number at the top).

### The `alias` Command

The `alias` command is a "superpower" for efficiency. It allows you to create short, memorable shortcuts for long or complex commands that you find yourself typing frequently.

**Basic Usage:**
`alias nickname='the long command here'`

**Examples from my workflow:**

- `alias add='git add .'`
- `alias commit='git commit -m'`
- `alias push='git push'`

**Temporary vs. Permanent:**

- **Temporary:** If you type an alias directly into the terminal, it only lasts for that specific session. Once you close the terminal, the alias is gone.
- **Permanent:** To make an alias work every time you open a terminal, you must add it to your shell's configuration file. For the Bash shell, this is usually located at `~/.bashrc`.
  1. Open the file: `nano ~/.bashrc`
  2. Add your alias lines at the bottom.
  3. Save and exit.
  4. Apply changes: `source ~/.bashrc`

**Removing an Alias:**
If you want to delete a shortcut during your current session, use the `unalias` command:
`unalias nickname`

**Pro-Tip:**
You can type simply `alias` (without any arguments) to see a list of all shortcuts currently active in your system. This is great for checking if a nickname is already taken!
