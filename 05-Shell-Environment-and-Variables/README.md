# Shell Environment and Variables

### The `$USER` Variable

Environment variables act like global settings for your Linux system. The current shell session reads them to determine how to behave and how to handle user preferences.

- **Purpose:** Holds the username of the user currently logged into the session.
- **Usage:** You can view its value by prefixing the variable name with a dollar sign (`$`) using the `echo` command:
  `echo $USER`
- **Combining with text:**
  `echo "Hello $USER"`

### The `$SHELL` Variable

This variable stores the path to your default login shell (e.g., `/bin/bash` or `/bin/zsh`).

- **Concept:** The shell is an interpreter that processes your text commands, executes them, and displays the output on your terminal screen. Different shells (like Bash, Zsh, or Fish) offer distinct features and syntax rules.
- **Usage:**
  `echo $SHELL`
- **Combining with text:**
  `echo "My current shell is $SHELL"`

> **Technical Note:** `$SHELL` points to your **default login shell** defined in `/etc/passwd`. It does not automatically change if you temporarily switch to another shell inside your terminal (like launching `zsh` from inside `bash`). To check the _currently active process_, you can use `echo $0`.

### The `env` Command

The `env` command allows you to view all exported environment variables currently available in your shell session (such as `USER`, `SHELL`, `PATH`, `HOSTNAME`, and `HOME`).

- **Viewing variables:** Simply run `env` to print the full list.
- **Setting variables temporarily:** Running `env VAR=value command` allows you to set or override a variable for a single command execution without affecting the rest of your session:
  `env LANG=C date`

### The `$PATH` Variable

Commands in Linux are simply executable programs or binaries (often compiled from C, Go, or written as shell scripts).

Normally, to run a program, Linux requires its exact file location (e.g., `/usr/bin/ls`). However, typing full paths every time would be tedious. The **`$PATH`** environment variable solves this by storing a colon-separated (`:`) list of directories where the shell automatically searches for executable files.

- **How it works:** When you type `ls`, the shell searches each directory listed in `$PATH` from left to right until it finds an executable named `ls`.
- **If not found:** The shell returns the error: `command not found`.
- **View your current path:**
  `echo $PATH`
  _(Example output: `/usr/local/bin:/usr/bin:/bin:/usr/sbin`)_

### The `which` Command

The `which` command helps you pinpoint the exact binary file that executes when you type a command.

- **Basic Usage:**
  `which ls`
  _(Output: `/usr/bin/ls`)_
- **Checking for Aliases or Multiple Locations:**
  By default, `which` only shows the first matching executable found in your `$PATH`. Using the **`-a`** flag tells it to display **all** matching executables in your path.
  `which -a ls`

> **Note on Shell Built-ins & Aliases:** Some commands (like `cd` or `echo`) are built directly into the shell process itself rather than stored as external binary files on disk. For shell built-ins or aliases, `type command_name` (e.g., `type cd` or `type ls`) often provides more detailed information than `which`.

### Executing Your Own Programs (`./script.sh`)

If you create a script in your current directory (e.g., `my_script.sh`), typing `my_script.sh` in the terminal will fail with `command not found`. This happens because **the current working directory (`.`) is not included in `$PATH` by default** for security reasons.

To run an executable from your current directory, you must specify its relative path:

```bash
./my_script.sh
```

. = Current directory

/ = Directory separator

my_script.sh = File name

Prerequisite: For a script to run this way, it must have executable permissions enabled (e.g., via chmod +x my_script.sh). Alternatively, you can add your custom script folder to your $PATH variable in ~/.bashrc: export PATH="$PATH:/path/to/my/scripts".

# Creating and Exporting Shell Variables

### Creating Local Variables

You can define variables in the shell using a key-value syntax.

**Syntax:**
`NAME="bassem"`
`AGE=25`

**Rules:**

- **No spaces around the `=` sign:** Writing `NAME = "bassem"` or `NAME= "bassem"` will cause a syntax error, as the shell will attempt to run `NAME` as a command.
- **Quotes:** Strings containing spaces or special characters must be wrapped in single (`'`) or double (`"`) quotes. Numbers do not require quotes.

### Reading Variables

To access or print the value of a variable, prefix its name with a dollar sign (`$`):

`echo $NAME`

**Double Quotes vs. Single Quotes:**

- **Double quotes allow variable expansion:**
  `echo "Hello, $NAME"` -> Output: `Hello, bassem`
- **Single quotes treat text as a literal string:**
  `echo 'Hello, $NAME'` -> Output: `Hello, $NAME`

### Exporting Variables (`export`)

By default, variables created in a shell session are **local variables**. They are only accessible within that specific shell process and are **not inherited by child processes** (scripts, subshells, or applications launched from that terminal).

To convert a local variable into an **environment variable** so child processes can access it, use the `export` command:

`export NAME`

Alternatively, you can create and export a variable in a single line:
`export API_KEY="secret_token_123"`

### Why `export` Matters in DevOps

1. **Secrets & Configurations:** Used to pass sensitive information (like API tokens, database passwords, or secret keys) to applications without hardcoding them into source code.
2. **Environment Isolation:** Allows you to change application behavior (e.g., `export ENV="production"` vs. `export ENV="development"`) dynamically before running deployment or build tools.

> **Scope Note:** An exported variable is passed down to **child processes** (sub-processes spawned _from_ that session). It does **not** persist across separate terminal sessions or pass up to parent processes unless saved in a startup file like `~/.bashrc`.

# Permanent Shell Configuration (`.bashrc` / `.zshrc`)

Edit your shell's **`rc`** (run commands) file to keep environment variables, aliases, and custom paths active across all future terminal sessions.

### Key Files

- **Bash:** `~/.bashrc`
- **Zsh:** `~/.zshrc`

---

### Cheat Sheet

```bash
# 1. Edit config file
nano ~/.bashrc       # or ~/.zshrc

# 2. Add your aliases/variables at the bottom
alias ll='ls -la'
export MY_VAR="permanent_value"

# 3. Reload config immediately (without restarting terminal)
source ~/.bashrc     # or source ~/.zshrc
```

# Command Substitution `$(...)`

Executes a command inside `$(...)` and replaces it with its output to pass as an argument to another command.

### Cheat Sheet

```bash
# Basic Syntax
command $(subcommand)

# Examples
mkdir $(date +%F)         # Creates a directory named with today's date (e.g., 2026-10-01)
echo "User: $(whoami)"    # Embeds command output inside string
kill -9 $(pgrep nginx)    # Finds PID of nginx and kills it directly
```

> **Note:** The older syntax uses backticks `command`, but `$(...)` is preferred because it supports easy nesting:
