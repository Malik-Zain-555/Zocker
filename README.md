# 🐯 Zocker
**Zocker** is a tiny, Python-powered, Docker-inspired container manager—perfect for learners, tinkerers, and anyone who wants to organize code projects with playful isolation!  
Zocker does **not** use OS-level containers, but simulates them as folders, giving you a Docker-flavored workflow—no root required, no risk, no setup hassle.

---

## ⭐ Use Cases: Why Use Zocker?

- **Learn container concepts**: Practice a Docker-like workflow safely, without installing heavy dependencies or needing admin rights.
- **Organize your code playgrounds**: Isolate C++/Python/JavaScript projects as self-contained, transportable “containers”.
- **Export/import project environments**: Share your project as a tarball, re-import on any Unix-like system!
- **Classrooms and demos**: Teach CI/container basics, run coding exercises, and manage project shells for beginners.
- **No risk “sandbox”**: Since everything happens in local folders, mistakes don’t affect the rest of your computer.

---

## 🚀 Installation

1. Download or clone this repo:

   ```bash
   git clone https://github.com/Malik-Zain-555/Zocker.git
   cd Zocker
   ```

2. Make `zocker` executable if needed:

   ```bash
   chmod +x zocker
   ```

3. Optionally add to your PATH:

   ```bash
   export PATH="$PWD:$PATH"
   ```

4. Try it!  
   Running without arguments prints:

   ```bash
   zocker
   # Output: No command.
   ```

---

## 🛠 Requirements

- Python 3.x  
- Unix-like system (Linux, macOS, WSL)
- Basic tools: bash, g++, python3, node, w3m, tar

---

## 📁 How it Works: Storage Layout

```
~/.zocker/
  containers/
    <random-name>/
      meta.json    # metadata (name, status, runner, fake pid, etc.)
      rootfs/      # your project files
      logs.txt     # everything run in shell, with output/errors
```

Containers have random names (`happy-tiger`, `crazy-panda`).  
Each container is isolated from the others!

---

## 📋 Supported File Types & Automatic Runners

| Extension | Runner    | Command             |
|:---------:|:----------|:--------------------|
| `.cpp`    | g++       | ./app (compiled)    |
| `.py`     | python3   | python3 <file>      |
| `.js`     | node      | node <file>         |
| `.sh`     | bash      | bash <file>         |
| `.html`   | w3m       | w3m <file>          |

You can use **manual runner** for other interpreters too!

---

## 🎮 Full Command Reference

### 1. `build` — Auto-Detect and Build Container

**Usage:**  
```bash
zocker build <file>
```

Creates a container from a source file, auto-detects the runner by file extension, and sets up everything.
- Copies `<file>` into `rootfs`.
- Compiles or sets up command.
- Container is named randomly.

**Example:**  
```bash
zocker build main.cpp
# Output: Container Name: sleepy-wolf, Default Command: ./app
```

Use it for quick, one-file project isolation or easy testing.

---

### 2. `create` — Build Container With Explicit Runner

**Usage:**  
```bash
zocker create <runner> <file>
```

Same as build, but you manually specify which interpreter or compiler to use.
- Use if auto-detect doesn't support your tool, or you want full control.

**Example:**  
```bash
zocker create python3 my_script.py
zocker create node app.js
```

Useful for advanced workflows or custom environments!

---

### 3. `run` — Start Container & Enter Interactive Shell

**Usage:**  
```bash
zocker run <container-name>
```

- Starts an interactive shell (`rootfs` as working dir).
- Assigns a fake PID if needed.
- Status set to "running".
- Shell prompt is colored with the container name!

**Example:**  
```bash
zocker run happy-tiger
# Output: Entering container shell: happy-tiger
```

**Inside the shell** you can use these built-ins:
- `help` — Show command help.
- `whoami` — Print container name.
- `ls` — List files.
- `cat <file>` — Read a file.
- `run <file>` — Auto-compile/run a file (see runners above).
- `pwd`, `clear`, `exit` — Standard shell commands.

*Also supports regular Linux commands (they run inside rootfs).*

---

### 4. `ps` — List All Containers

**Usage:**  
```bash
zocker ps
```

Shows a neatly formatted table of all containers:

| Name          | PID    | Status   | Command            |
|---------------|--------|----------|--------------------|
| sleepy-wolf   | 20324  | running  | python3 hello.py   |

States:
- `created`, `built`, `running`, `stopped`

Perfect for monitoring your local “Zocker zoo”.

---

### 5. `stop` — Stop a Running Container

**Usage:**  
```bash
zocker stop <container-name>
```

- Sets status to "stopped", clears fake PID.
- Container must be running.

**Example:**  
```bash
zocker stop crazy-panda
```
Use when done working with a running container.

---

### 6. `stop-all` — Stop All Running Containers

**Usage:**  
```bash
zocker stop-all
```

Skims through all containers and stops any "running" container.  
Best for classroom resets or mass shutdowns.

---

### 7. `rm` — Remove (Delete) a Container

**Usage:**  
```bash
zocker rm <container-name>
```

- Container must be "stopped".
- Deletes all data for that container.

**Example:**  
```bash
zocker rm sleepy-wolf
```

Warning: This is irreversible!

---

### 8. `rm-all` — Remove All Stopped Containers

**Usage:**  
```bash
zocker rm-all
```

Bulk delete everything that’s safely stopped.  
Perfect for cleaning up after demos or coding sessions.

---

### 9. `export` — Archive Container as `.tar`

**Usage:**  
```bash
zocker export <container-name> <output.tar>
```

- Saves all container data as a `.tar` file.
- Great for sharing or backing up environments.

**Example:**  
```bash
zocker export angry-lion angry-lion.tar
```

---

### 10. `import` — Restore Container from `.tar`

**Usage:**  
```bash
zocker import <file.tar>
```

- Unpacks everything into a new random-named container.
- Container picked up in `ps`.

**Example:**  
```bash
zocker import angry-lion.tar
```

Use this to move your environment between computers!

---

### 11. `logs` — Show Command Output Log from a Container

**Usage:**  
```bash
zocker logs <container-name>
```

Prints all output/errors produced inside the interactive shell of that container.

**Example:**  
```bash
zocker logs happy-tiger
```

Handy for debugging or grading student work!

---

### 12. `inspect` — Display Raw Container Metadata

**Usage:**  
```bash
zocker inspect <container-name>
```

Prints the JSON config (name, runner, command, PID, etc).  
Useful for auditing and teaching.

**Example:**  
```bash
zocker inspect crazy-panda
```

---

## 💡 Example Workflows

### A. What You’d Do as a Student

```bash
# Build a container for your script:
zocker build assignment.py
# Start shell and run it:
zocker run wild-shark
wild-shark > run assignment.py
wild-shark > exit

# Stop and remove when graded:
zocker stop wild-shark
zocker rm wild-shark
```

### B. Sharing Projects or Grading

```bash
# Export your coded testbed:
zocker export sleepy-wolf wolf.tar

# On another computer:
zocker import wolf.tar
zocker ps
zocker run crazy-eagle
crazy-eagle > run assignment.py
```

---

## ⚡ Limitations & Safety

- **No real process isolation** (it’s for learning, not security!)
- **Fake PIDs, no networking**
- **Just folders and shell commands—for pure education & experimentation**

You won’t break your system, and you can clean up your playground anytime.

---

## 🏅 License & Credit

MIT License  
Created & maintained by [Malik-Zain-555](https://github.com/Malik-Zain-555)

---

## 🦁 In Short

**Zocker = containers for everyone, everywhere.**  
Perfect learning tool for classrooms, self-study, or quick code isolation.  
Try it—see what “containers” _feel_ like!

Happy Hacking! 🐾
