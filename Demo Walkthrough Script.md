# Demo Walkthrough Script
### "Show them you can use it" — Jupyter · Python · Scripts · Unix · CLI

> One connected flow. You'll open a terminal, navigate around (Unix + CLI), write and run a Python script, then do the same interactively in Jupyter. Follow the steps in order. The **Say:** lines are optional narration so you sound confident while you work.
>
> Assumes Python is installed. Test first: open a terminal and type `python3 --version` (Windows: `python --version`). If you see a version number, you're good.

---

## PART 1 — Unix + CLI (open a terminal, get comfortable)

> On Mac/Linux use **Terminal**. On Windows use **PowerShell**, **Git Bash**, or **WSL**. Commands below are Mac/Linux; Windows notes are in parentheses.

**Step 1 — See where you are**
```bash
pwd
```
> **Say:** "First I'll check my current directory."

**Step 2 — Make a working folder and enter it**
```bash
mkdir demo
cd demo
```
> **Say:** "I'll create a folder for the demo and move into it." (This shows `mkdir` + `cd` — core Unix navigation.)

**Step 3 — Confirm you're in it, and that it's empty**
```bash
pwd
ls -la
```
> **Say:** "`ls -la` lists everything including hidden files."

**Step 4 — Create a small data file from the command line**
```bash
echo "apple
banana
apple
cherry
banana
apple" > fruits.txt
```
(Windows PowerShell: use the same `echo ... > fruits.txt` — it works.)
> **Say:** "I'll write a quick text file using output redirection with `>`."

**Step 5 — Look at the file a few different ways**
```bash
cat fruits.txt
wc -l fruits.txt
grep apple fruits.txt
```
> **Say:** "`cat` prints it, `wc -l` counts the lines, and `grep` finds every line containing 'apple'." (This is your Unix text-tools moment — very impressive in a quick demo.)

**Step 6 — Chain commands with a pipe (the showstopper)**
```bash
sort fruits.txt | uniq -c | sort -nr
```
> **Say:** "I'll pipe commands together to count how many times each fruit appears, sorted by frequency." (Pipes = instant "this person knows the CLI".)

---

## PART 2 — Write and run a Python Script

**Step 7 — Create the script**
Open a text editor and save this as `count_fruits.py` inside the `demo` folder. (Or create it from the terminal — see the tip below.)
```python
#!/usr/bin/env python3
"""Count how many times each fruit appears in fruits.txt."""

# Read the file
with open("fruits.txt") as f:
    fruits = [line.strip() for line in f if line.strip()]

# Count them using a dictionary
counts = {}
for fruit in fruits:
    counts[fruit] = counts.get(fruit, 0) + 1

# Print results, most common first
for fruit, n in sorted(counts.items(), key=lambda x: -x[1]):
    print(f"{fruit}: {n}")
```
> **Tip (create it without an editor):** you can paste it straight from the terminal:
> ```bash
> cat > count_fruits.py
> # (paste the code, then press Enter, then Ctrl+D to save)
> ```

> **Say while writing:** "I'll read the file into a list, count each fruit with a dictionary using `.get()` so I don't hit a KeyError, then print them sorted by count with an f-string."

**Step 8 — Run the script from the CLI**
```bash
python3 count_fruits.py
```
(Windows: `python count_fruits.py`)
> **Say:** "Now I run the script from the command line." Expected output:
> ```
> apple: 3
> banana: 2
> cherry: 1
> ```

**Step 9 — Redirect the script's output to a file, then check it**
```bash
python3 count_fruits.py > results.txt
cat results.txt
```
> **Say:** "I can redirect the output to a file and read it back." (Ties Python + CLI + Unix together in one line — a strong beat.)

**Optional flex — make it directly executable (Unix concept)**
```bash
chmod +x count_fruits.py
./count_fruits.py
```
> **Say:** "Because it has a shebang line, I can make it executable and run it directly." (Skip on Windows.)

---

## PART 3 — Jupyter Notebook (same logic, interactively)

**Step 10 — Launch Jupyter**
```bash
jupyter notebook
```
(If not installed, `pip install notebook` first — or use **VS Code**, which opens `.ipynb` files natively, or [Google Colab](https://colab.research.google.com) in a browser with nothing to install. Colab is the safest fallback if you're unsure.)
> **Say:** "Now I'll do the same thing interactively in Jupyter to show the exploratory workflow."

**Step 11 — New notebook, first cell: a Markdown heading**
Change the cell type to **Markdown** (press `Esc` then `M`), type:
```markdown
# Fruit Counter Demo
Counting fruit occurrences interactively.
```
Run it with **`Shift+Enter`**.
> **Say:** "Markdown cells let me document my work alongside the code."

**Step 12 — Code cell: read the file**
```python
with open("fruits.txt") as f:
    fruits = [line.strip() for line in f if line.strip()]
fruits
```
Run with **`Shift+Enter`**.
> **Say:** "I'll read the file into a list — notice the notebook shows the output right under the cell."

**Step 13 — Next cell: count with a comprehension-friendly tool**
```python
from collections import Counter
counts = Counter(fruits)
counts
```
> **Say:** "`Counter` from the standard library counts everything in one line — this is where Jupyter shines for quick exploration."

**Step 14 — Next cell: a tiny visualization (optional but memorable)**
```python
%matplotlib inline
import matplotlib.pyplot as plt

plt.bar(counts.keys(), counts.values())
plt.title("Fruit counts")
plt.show()
```
> **Say:** "A magic command renders the plot inline. This is the payoff of using a notebook over a plain script." (If matplotlib isn't installed, skip — the count itself is enough.)

**Step 15 — Show you understand the kernel**
Go to **Kernel → Restart & Run All**.
> **Say:** "Restarting the kernel and running all cells top to bottom proves the notebook is reproducible and doesn't rely on hidden state." (This one line makes you sound experienced.)

---

## The 30-second version (if they rush you)
1. `mkdir demo && cd demo` — Unix/CLI navigation
2. `echo "..." > fruits.txt` then `sort fruits.txt | uniq -c` — Unix text tools + pipe
3. `python3 count_fruits.py` — run a Python script from the CLI
4. Open the same file in Jupyter, use `Counter`, `Shift+Enter` through cells, **Restart & Run All**

That hits all five in under a minute.

---

## Pointers so you look fluent
- **Use tab-completion** — start typing `count_` then press **Tab**. Never fully type long filenames; it signals comfort.
- **Up arrow** to recall your last command instead of retyping.
- **Narrate lightly** — say what you're about to do *before* you do it, not after.
- **If something errors, stay calm** — read the error out loud, fix it, move on. Recovering smoothly looks *better* than a flawless run.
- **Don't over-explain.** They want to see you *do* it, not lecture. Short sentences.
- **Have the terminal font size bumped up** so they can read your screen.

You've got this. 🚀
