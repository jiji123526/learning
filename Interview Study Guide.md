# Technical Interview Study Guide
### Jupyter Notebook · Python · Scripts · Unix · CLI

> A focused, practical review. Read top to bottom, then use the **Rapid-Fire Q&A** at the end to self-test. Aim to *explain out loud* — interviewers care about how you reason, not just the right keyword.

---

## 1. Python

### Core concepts they'll probe
- **Data types**: `int`, `float`, `str`, `bool`, `list`, `tuple`, `dict`, `set`, `None`
- **Mutable vs immutable**: lists/dicts/sets are mutable; strings/tuples/ints are immutable. This matters for function arguments and dict keys (keys must be immutable/hashable).
- **List vs tuple vs set vs dict**:
  - `list` — ordered, mutable, allows duplicates → `[1, 2, 2]`
  - `tuple` — ordered, immutable → `(1, 2)`
  - `set` — unordered, unique elements, fast membership test → `{1, 2}`
  - `dict` — key→value pairs, keys unique & hashable → `{"a": 1}`
- **Comprehensions** (very common ask):
  ```python
  squares = [x**2 for x in range(10)]
  evens   = [x for x in range(20) if x % 2 == 0]
  lookup  = {word: len(word) for word in ["hi", "there"]}
  ```
- **Functions**: default args, `*args` / `**kwargs`, return values, scope (local vs global).
- **Error handling**:
  ```python
  try:
      result = risky()
  except ValueError as e:
      print(f"bad value: {e}")
  finally:
      cleanup()
  ```
- **f-strings**: `f"{name} scored {score:.2f}"` — know the format specifiers.

### Things people forget under pressure
- `is` checks **identity** (same object), `==` checks **equality** (same value). Use `==` for value comparison; use `is` only for `None`/singletons.
- Copying: `b = a` copies the *reference*, not the data. Use `a.copy()` or `copy.deepcopy()` for nested structures.
- Default mutable argument trap:
  ```python
  def f(x, acc=[]):   # BUG: acc is shared across calls
      acc.append(x); return acc
  # Fix: use acc=None, then acc = acc or []
  ```
- `enumerate()` for index+value, `zip()` to iterate two lists together.
- Dict methods: `.get(key, default)` avoids KeyError; `.items()`, `.keys()`, `.values()`.

### If the role touches data (pandas/numpy)
- `pd.read_csv()`, `df.head()`, `df.info()`, `df.describe()`
- Filtering: `df[df["age"] > 30]`; selecting: `df.loc[]` (labels) vs `df.iloc[]` (positions)
- `groupby().agg()`, `merge()`, handling `NaN` with `.fillna()` / `.dropna()`

---

## 2. Jupyter Notebook

### What it is
An interactive environment that runs code in **cells**, keeping state in memory between cells. Great for exploration, data analysis, and showing work step by step.

### Must-know mechanics
- **Cell types**: Code cells (run Python) and Markdown cells (notes/headings).
- **Run order matters**: cells share one kernel/namespace. Running cells out of order is the #1 source of confusing bugs — variables can hold stale values. `Kernel → Restart & Run All` gives a clean, reproducible run.
- **The kernel** is the process executing your code. Restarting it clears all variables.
- **Execution counter** `In [3]:` shows the order cells actually ran — not their position on the page.

### Handy shortcuts (command mode — press `Esc` first)
| Key | Action |
|-----|--------|
| `Shift+Enter` | Run cell, select next |
| `Ctrl+Enter` | Run cell, stay |
| `A` / `B` | Insert cell above / below |
| `DD` | Delete cell |
| `M` / `Y` | To Markdown / to Code |

### Magic commands
- `%timeit some_code()` — benchmark a line
- `%matplotlib inline` — render plots in the notebook
- `!ls -la` or `!pip install x` — run a **shell command** from inside a cell (ties directly into the CLI section!)

### Interview-favorite gotcha
> *"Why might a notebook that works for you fail for a colleague?"*
Answer: hidden state from out-of-order execution, un-pinned dependencies, or a variable defined in a cell that was later deleted. Fix: Restart & Run All, and pin dependencies.

---

## 3. Scripts (writing reusable Python programs)

### Structure of a real script
```python
#!/usr/bin/env python3
"""One-line description of what this does."""
import argparse

def main(name, count):
    for _ in range(count):
        print(f"Hello, {name}")

if __name__ == "__main__":
    parser = argparse.ArgumentParser()
    parser.add_argument("name")
    parser.add_argument("--count", type=int, default=1)
    args = parser.parse_args()
    main(args.name, args.count)
```

### Key ideas
- **`if __name__ == "__main__":`** — code here runs only when the file is executed directly, *not* when imported. Classic interview question: *"What does this do and why use it?"*
- **Shebang** `#!/usr/bin/env python3` — lets you run `./script.py` directly (after `chmod +x`).
- **`argparse`** — the standard way to accept command-line arguments cleanly.
- **`sys.argv`** — raw list of args (`sys.argv[0]` is the script name).
- **Exit codes** — `sys.exit(0)` = success, non-zero = failure. Scripts should return meaningful exit codes so the shell/CLI can react.
- **Reading input**: from a file, from `stdin` (`sys.stdin`), or from args.
- **Logging over print** for real scripts: `import logging`.

### Notebook → Script
Be ready to explain *why* you'd convert a notebook to a script: automation, scheduling, version control, reproducibility, running without a UI.

---

## 4. Unix / Linux

### Filesystem & navigation
| Command | Does |
|---------|------|
| `pwd` | print working directory |
| `ls -la` | list all files, long format |
| `cd /path` | change directory (`cd ..` up, `cd ~` home, `cd -` previous) |
| `mkdir -p a/b/c` | make nested dirs |
| `cp`, `mv`, `rm` | copy, move/rename, remove (`rm -r` recursive, `rm -f` force) |
| `find . -name "*.py"` | search for files |
| `cat`, `less`, `head -n 20`, `tail -f` | view files (`tail -f` follows a live log) |

### Paths & permissions
- **Absolute** path starts at `/`; **relative** path starts from where you are.
- `~` = home dir, `.` = current, `..` = parent.
- Permissions: `rwx` for **user / group / other**. `chmod +x file` makes it executable; `chmod 755` = rwxr-xr-x.
- `chown user:group file` changes ownership.

### Text processing power tools (interviewers love these)
- **`grep`** — search text: `grep -i "error" log.txt` (`-i` ignore case, `-r` recursive, `-n` line numbers, `-v` invert)
- **`|` pipe** — send output of one command into another: `cat log.txt | grep ERROR | wc -l`
- **Redirection** — `>` overwrite, `>>` append, `<` input, `2>` stderr: `python script.py > out.txt 2> err.txt`
- **`wc`** — count lines/words/chars (`wc -l`)
- **`sort`**, **`uniq`** (usually `sort | uniq -c` to count duplicates)
- **`awk`** / **`sed`** — field extraction and stream editing (know they exist; `awk '{print $1}'` prints first column, `sed 's/old/new/g'` substitutes)
- **`cut -d',' -f1`** — split by delimiter, take a field

### Processes & environment
- `ps aux`, `top`/`htop` — see running processes
- `kill <PID>`, `kill -9 <PID>` — stop a process
- `&` runs in background; `Ctrl+C` interrupts; `Ctrl+Z` suspends
- **Environment variables**: `echo $PATH`, `export VAR=value`, `$HOME`
- `which python3` — find which executable runs

---

## 5. CLI (command-line interface / shell workflow)

The CLI *is* how you drive Unix — this section is about fluency and the tools around it.

### Shell essentials
- **Tab completion** — autocomplete files/commands (mention this; shows fluency).
- **Up arrow / `history`** — recall past commands; `!!` reruns last; `Ctrl+R` searches history.
- **Chaining**: `&&` (run next only if previous succeeded), `||` (run next only if previous failed), `;` (run regardless).
  - `mkdir build && cd build`
- **Wildcards / globbing**: `*` any chars, `?` single char, `[abc]` set → `rm *.tmp`

### Python virtual environments (huge in interviews)
```bash
python3 -m venv .venv          # create isolated env
source .venv/bin/activate      # activate (Windows: .venv\Scripts\activate)
pip install -r requirements.txt
deactivate                     # leave the env
```
- **Why venvs?** Isolate project dependencies so versions don't clash between projects.
- `pip freeze > requirements.txt` — capture exact versions for reproducibility.

### Running Python from the CLI
- `python3 script.py --count 3 Alice`
- `python3 -m module_name` — run a module as a script
- `python3 -c "print(2+2)"` — one-liner
- `python3 -i script.py` — run then drop into interactive shell

### Git (often bundled with "CLI" expectations)
`git status`, `git add .`, `git commit -m "msg"`, `git push`, `git pull`, `git log --oneline`, `git branch`, `git checkout -b feature`, `git diff`. Know the **add → commit → push** flow and what a branch is.

---

## Rapid-Fire Q&A (self-test — cover the answers)

1. **Difference between a list and a tuple?**
   List is mutable, tuple is immutable. Tuples can be dict keys / set members; lists cannot.

2. **What does `if __name__ == "__main__":` do?**
   Runs the block only when the file is executed directly, not when imported as a module.

3. **`==` vs `is`?**
   `==` compares values; `is` compares object identity. Use `is` only for `None`.

4. **How do you count how many lines contain "ERROR" in a log?**
   `grep -c "ERROR" log.txt` or `grep "ERROR" log.txt | wc -l`.

5. **What's a pipe and give an example?**
   `|` passes one command's stdout to the next command's stdin: `ps aux | grep python`.

6. **Why use a virtual environment?**
   To isolate a project's dependencies and versions from other projects and the system Python.

7. **Your Jupyter notebook gives different results than a teammate's — why?**
   Out-of-order cell execution / hidden state / unpinned dependencies. Fix: Restart & Run All, pin deps.

8. **How do you make a script accept command-line arguments?**
   Use `argparse` (or read `sys.argv`).

9. **`>` vs `>>`?**
   `>` overwrites the file; `>>` appends to it.

10. **How do you make a file executable and run it?**
    `chmod +x script.py` then `./script.py` (needs a shebang line).

11. **How do you find all `.py` files under the current directory?**
    `find . -name "*.py"`.

12. **What is the kernel in Jupyter?**
    The process that executes your code and holds all variable state; restarting it clears everything.

13. **Mutable default argument — what's the bug?**
    A default like `def f(x, acc=[])` shares one list across all calls. Use `acc=None` and create inside.

14. **`grep -r "TODO" .` — what does it do?**
    Recursively searches all files in the current directory tree for the text "TODO".

15. **How do you chain "make dir then enter it, only if the first works"?**
    `mkdir build && cd build`.

---

## Study plan for the next few days
1. **Type every code snippet yourself** in a terminal + a notebook — don't just read.
2. **Practice explaining out loud** — pick 5 Rapid-Fire questions and answer as if to an interviewer.
3. **Build one tiny end-to-end example**: write a Python script that reads a file, run it from the CLI with an argument, redirect output to a file, then `grep` that output. This single exercise touches all five topics.
4. **Have 1–2 concrete stories ready**: a time you debugged something in a notebook, or automated a task with a script.

Good luck — you've got this. 🚀
