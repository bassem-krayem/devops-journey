# Processes and Job Control

### 1. Viewing Processes (`ps` & `top`)

- `ps aux` -> Detailed list of all running processes.
- `ps aux | grep "name"` -> Find a process by name to get its PID.
- `top` -> Interactive process viewer.
  - `Shift + M` -> Sort by Memory usage.
  - `Shift + P` -> Sort by CPU usage.
  - `q` -> Exit.

---

### 2. Job Control (`&`, `Ctrl+Z`, `jobs`, `fg`)

- `<command> &` -> Start a command directly in the background.
- `Ctrl + Z` -> Pause/suspend current running process.
- `jobs` -> List active jobs in the current shell session.
- `fg %<job_num>` -> Bring a background job to the foreground.

---

### 3. Killing Processes (`kill`, `pkill`)

- `kill <PID>` -> Terminate gracefully (`SIGTERM` / signal 15).
- `kill -9 <PID>` -> Force kill immediately (`SIGKILL` / signal 9).
- `pkill -f <name>` -> Kill process by name matching pattern.

---

### 4. Process Priority / Niceness (`nice` & `renice`)

Priority scale ranges from **-20** (highest priority) to **19** (lowest priority / "nicest"). Default is `0`.

- `ps -l` or `ps -o pid,ni,comm` -> View process PID, Niceness (`NI`), and Command name.
- `nice -n 10 sleep 4000 &` -> Start a process with custom niceness (`10`).
- `renice -n 5 -p <PID>` -> Change niceness of an already running process to `5`.
