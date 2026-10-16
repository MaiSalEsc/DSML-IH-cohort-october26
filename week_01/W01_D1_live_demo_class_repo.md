# 🛠️ Week 1 · Class 1 — Live demo: creating our class repository

**Goal (40 min):** build, live and from zero, the repository we use for the **whole bootcamp**: `DSML-IH-cohort-october26`.
Along the way we practise the terminal, a Python virtual environment with `uv`, Git, GitHub and pull requests.

**At the end, the repo looks like this:**

```
DSML-IH-cohort-october26/
├── README.md          ← front page of the repo
├── .gitignore         ← what Git must NOT upload
├── pyproject.toml     ← list of Python packages (created by uv)
├── uv.lock            ← exact package versions (created by uv)
├── data/              ← datasets used in class (kept local, ignored by Git)
├── week_01/           ← notebooks of week 1
├── week_02/
│   ...
└── week_24/
```

**What we practise:** `pwd` · `ls` · `cd` · `mkdir` · `touch` · `echo` · `cat` · `uv init` · `uv venv` · `source` · `uv add` · `uv sync` · `git init` · `git status` · `git add` · `git commit` · `git log` · `git remote` · `git push` · `git pull` · `git switch` · pull requests.

> 💻 **Mac:** use the **Terminal** app.
> 🪟 **Windows:** use **Git Bash** (installed with Git). Every command below is then **the same as on Mac**. Where Windows differs, a 🪟 line shows it.
> 🔀 GitHub calls a merge request a **pull request (PR)**.

---

## ⏱️ Timeline

| Time | Step | What students see |
|---|---|---|
| 0–3' | 0 · Check the setup | Git and uv installed, Git configured |
| 3–8' | 1 · Create the project with the terminal | a folder with a `README.md` |
| 8–14' | 2 · Python environment with uv | `.venv` active, pandas installed |
| 14–18' | 3 · `.gitignore` + first commits | `git log` shows the first commits |
| 18–23' | 4 · Put the repo on GitHub | the repo online, without `.venv` |
| 23–26' | 5 · Change the README on GitHub, pull it | the change arrives on the laptop |
| 26–34' | 6 · Branch → pull request → merge: the week folders | `data/` and `week_01 … week_24` on GitHub |
| 34–40' | 7 · How we use the repo every class | students clone it and run `uv sync` |

---

## ✅ Before class (send to students the day before)

1. Install **Git**: [git-scm.com/downloads](https://git-scm.com/downloads). Mac: type `git --version` in Terminal; if Git is missing, macOS offers to install it. Windows: run the installer with the default options (installs **Git Bash** and the **Git Credential Manager** for the GitHub login).
2. Install **uv** (Python package manager):
   - Mac: `curl -LsSf https://astral.sh/uv/install.sh | sh`
   - 🪟 Windows (PowerShell): `powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"`
3. Create a free account on [github.com](https://github.com).
4. Install **VS Code**: [code.visualstudio.com](https://code.visualstudio.com). Mac: Cmd+Shift+P → *Shell Command: Install 'code' command in PATH*.
5. *(Recommended)* Install the GitHub CLI ([cli.github.com](https://cli.github.com)) and run `gh auth login` → GitHub.com → HTTPS → "Login with a web browser".

---

## Step 0 · Check the setup (0–3')

Open the terminal (Mac: Cmd+Space → "Terminal"; Windows: Start → "Git Bash").

```bash
git --version
uv --version
```

Tell Git who you are (once per computer; use your GitHub email):

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global init.defaultBranch main
git config --list
```

What each line does:
- `user.name` / `user.email` → your name and email are stamped on every commit you make.
- `init.defaultBranch main` → the first branch created by `git init` is called `main`.
  - Older Git versions use `master`, but GitHub uses `main`.
  - This way your laptop and GitHub use the same name, so `git push -u origin main` works later.
- `--global` → applies to **all** repos on this computer, so you run these commands only once.
- `git config --list` → shows all active Git settings, a quick check that the lines above worked. (Long list that "freezes"? Press `q`.)

> 🎤 **Teacher:** check that `user.name`, `user.email` and `init.defaultbranch=main` appear.

---

## Step 1 · Create the project with the terminal (3–8')

Where am I, and what is here?

```bash
pwd
ls
```

Create a bootcamp folder and the repo folder, and go inside:

```bash
cd ~
mkdir -p ironhack
cd ironhack
mkdir DSML-IH-cohort-october26
cd DSML-IH-cohort-october26
pwd
```

`pwd` shows e.g. `/Users/ana/ironhack/DSML-IH-cohort-october26` (Git Bash: `/c/Users/ana/ironhack/...`).

What `mkdir -p ironhack` means:
- `mkdir` = **m**a**k**e **dir**ectory → create a folder.
- `-p` does two things:
  - **No error if the folder already exists** (plain `mkdir ironhack` would say `File exists`). Useful because some of you may already have an `ironhack` folder.
  - **Creates parent folders too**: `mkdir -p ironhack/projects/week_01` creates all three levels at once.
- So `mkdir -p ironhack` = "make sure an `ironhack` folder exists".

Create the README with `touch`, then write into it:

```bash
touch README.md
echo "# DSML-IH-cohort-october26" > README.md
echo "" >> README.md
echo "Class repository of the Ironhack Data Science & Machine Learning bootcamp, October 2026 cohort." >> README.md
cat README.md
ls -la
```

- `>` writes a new file, `>>` adds a line at the end.
- Or with VS Code: `code .` → click `README.md` → type → save.

> 🎤 **Teacher:** `README.md` is Markdown, the same language as the text cells of our notebooks. GitHub shows it as the front page of the repo.

---

## Step 2 · Python environment with uv (8–14')

**Why?** Each project gets its own private Python and packages, so projects never break each other.

```bash
uv init --bare            # 1. creates pyproject.toml = the project's "ID card"
cat pyproject.toml
uv venv                   # 2. creates the environment in the hidden folder .venv/
ls -la
```

Activate it with `source`:

```bash
source .venv/bin/activate
which python              # python now lives inside .venv
```

- 🪟 Git Bash: `source .venv/Scripts/activate`
- 🪟 PowerShell: `.venv\Scripts\activate`
- The prompt now starts with `(DSML-IH-cohort-october26)` → the environment is **active**.

Install the first packages of the course:

```bash
uv add numpy pandas jupyter
cat pyproject.toml        # the packages are recorded here
python -c "import pandas; print(pandas.__version__)"
```

- `uv add` = install **and** write the package into `pyproject.toml` + `uv.lock`.
- `deactivate` = leave the environment.
- `uv sync` = rebuild the exact same environment on another laptop (students will run this in Step 7).

---

## Step 3 · `.gitignore` + first commits (14–18')

The `.venv` folder is big and specific to each laptop: it must **never** go to GitHub. We tell Git **before** the first commit:

```bash
touch .gitignore
echo ".venv/" >> .gitignore
echo "__pycache__/" >> .gitignore
echo ".ipynb_checkpoints/" >> .gitignore
echo ".env" >> .gitignore
echo ".DS_Store" >> .gitignore
echo "data/*" >> .gitignore
echo "!data/.gitkeep" >> .gitignore
cat .gitignore
```

| Ignored | Why |
|---|---|
| `.venv/` | huge, rebuilt with `uv sync` |
| `__pycache__/` | Python cache |
| `.ipynb_checkpoints/` | Jupyter autosaves |
| `.env` | passwords / API keys |
| `.DS_Store` | macOS system file |
| `data/*` | datasets: can be large (GitHub blocks files > 100 MB) or private |
| `!data/.gitkeep` | `!` = **exception**: keep this one file, so the empty `data/` folder still exists on GitHub |

How the datasets rule works:
- `data/*` → ignore **everything inside** `data/`.
- `!data/.gitkeep` → **except** the placeholder file. The `!` means "do not ignore this one".
- ⚠️ Write `data/*`, not `data/`. With `data/` Git ignores the whole folder and the `!` exception no longer works.
- Datasets are shared another way (Google Drive link in the class materials). You download them into your local `data/` folder.
- Alternative: ignore only some file types anywhere in the repo, e.g. `*.csv`, `*.parquet`, `*.zip`.

What is `__pycache__/`?
- When you `import` a Python file (e.g. `utils.py`), Python first **translates** it into a faster format called *bytecode*.
- It saves that translation in a folder `__pycache__/` (files ending in `.pyc`), so next time it starts faster.
- Like a **pre-cooked meal**: it saves time, but you can always cook it again from the recipe (your `.py` file).
- Python creates it automatically, it's different on every laptop, and nobody reads it → no reason to put it on GitHub.

Turn the folder into a Git repository:

```bash
git init
git status
```

- Red = **untracked**. Notice: `.venv/` is **not** in the list → the `.gitignore` works.

Commit in two snapshots:

```bash
git add README.md .gitignore
git commit -m "Add README and .gitignore"

git add pyproject.toml uv.lock
git commit -m "Add uv project with numpy, pandas, jupyter"

git log --oneline
```

(If `git log` "freezes" the screen, press `q`.)

```
working folder ──git add──▶ staging area ──git commit──▶ history
```

---

## Step 4 · Put the repo on GitHub (18–23')

**On github.com:**
1. **+** (top right) → **New repository**.
2. Name: `DSML-IH-cohort-october26`.
3. Visibility: **Public**.
4. ⚠️ Leave everything else empty: no README, no .gitignore, no license (we already have them).
5. **Create repository**.

**In the terminal** (replace `YOUR-USERNAME`):

```bash
git remote add origin https://github.com/YOUR-USERNAME/DSML-IH-cohort-october26.git
git remote -v
git branch -M main
git push -u origin main
```

- `origin` = the name of our GitHub copy.
- `-u` = remember the link; next time just `git push`.
- Login: a browser window opens → authorise. (GitHub does not accept your password in the terminal.)

Refresh GitHub: README, `.gitignore`, `pyproject.toml`, `uv.lock` are online, **`.venv` is not**. 🎉

---

## Step 5 · Change the README on GitHub, then pull it (23–26')

This simulates a teammate changing the repo.

**On github.com:** open `README.md` → ✏️ **Edit** → add at the end:

```markdown
## Schedule
Lectures on Tuesday and Thursday, labs on Saturday.
```

→ **Commit changes…** → message `Add schedule` → *Commit directly to main* → **Commit changes**.

**In the terminal:**

```bash
cat README.md        # no "Schedule" yet
git pull
cat README.md        # now it's here
git log --oneline
```

> 🎤 **Teacher:** rule for teams: **`git pull` before you start, `git push` when you stop.**

---

## Step 6 · Branch → pull request → merge: the week folders (26–34')

Nobody changes `main` directly: we work on a **branch** and merge with a **pull request**.
Our change: create the `data/` folder and **one folder per week**.

**6.1 Branch (terminal):**

```bash
git switch -c add-course-structure
git branch            # * = current branch
```

**6.2 Create the folders:**

```bash
mkdir data
mkdir week_01 week_02 week_03
ls
```

24 weeks one by one is boring → a loop does it for us:

```bash
for i in $(seq -w 1 24); do mkdir -p week_$i; done
ls
```

- `seq -w 1 24` → `01 02 … 24`
- `-p` → no error if the folder already exists

Git does **not** track empty folders. Check:

```bash
git status            # "nothing to commit" — the folders are invisible to Git!
```

Put an empty placeholder file `.gitkeep` in each folder:

```bash
touch data/.gitkeep
for i in $(seq -w 1 24); do touch week_$i/.gitkeep; done
git status            # now Git sees them
```

Check that datasets really are ignored: create a fake dataset.

```bash
echo "name,age" > data/students.csv
echo "Ana,29" >> data/students.csv
git status -uall      # -uall lists every file: data/.gitkeep yes, students.csv NOT
git check-ignore -v data/students.csv   # tells you which .gitignore line ignores it
```

- `students.csv` stays on your laptop, it never goes to GitHub.

Document the structure in the README:

```bash
echo "" >> README.md
echo "## Structure" >> README.md
echo "- \`data/\` — datasets used in class (not on GitHub: download them from the class Drive into this folder)" >> README.md
echo "- \`week_01/\` … \`week_24/\` — notebooks coded live in class, one folder per week" >> README.md
cat README.md
```

Commit and push the branch:

```bash
git add .
git commit -m "Add data folder and week_01 to week_24 folders"
git push -u origin add-course-structure
```

**6.3 Switch branches and watch the folders appear and disappear:**

```bash
git switch main
ls                    # no week folders on main yet
git switch add-course-structure
ls                    # they are back
```

**6.4 Open the pull request (github.com):**
1. Yellow banner **"add-course-structure had recent pushes"** → **Compare & pull request**.
2. Title: `Add course structure`. Description: `data/ folder and one folder per week (1–24)`.
3. **Create pull request**.
4. Show the **Files changed** tab: this is where teammates review.

**6.5 Merge (github.com):**
1. **Merge pull request** → **Confirm merge**.
2. **Delete branch**.
3. Repo main page: `data/` and `week_01 … week_24` are on `main`.

**6.6 Update the laptop:**

```bash
git switch main
git pull
git log --oneline --graph
git branch -d add-course-structure
ls
```

---

## Step 7 · How we use the repo every class (34–40')

**Students, once (today):**

```bash
cd ~/ironhack
git clone https://github.com/YOUR-USERNAME/DSML-IH-cohort-october26.git
cd DSML-IH-cohort-october26
uv sync                        # creates .venv with the exact same packages
source .venv/bin/activate      # 🪟 Git Bash: source .venv/Scripts/activate
```

**Every class:**

```bash
cd ~/ironhack/DSML-IH-cohort-october26
git pull                       # get today's notebook in week_XX/
uv sync                        # new packages, if any
source .venv/bin/activate
jupyter lab                    # or open the folder in VS Code
```

- I (teacher) push each session's notebook to `week_XX/` before class.
- To avoid conflicts with my updates, **save your own version with a new name**, e.g. `W01_D2_python_basics_ana.ipynb`.

**Recap:**

```
 your laptop                                          GitHub
 touch / edit → git add → git commit → git push  →  main
                                                     │
       git pull  ←────────── edits on GitHub ────────┘
       git switch -c branch → commit → git push → Pull Request → Merge → git pull
```

---

## 🆘 Troubleshooting

| Problem | Fix |
|---|---|
| `uv: command not found` | Close and reopen the terminal after installing uv. |
| `.venv` appears in `git status` | `.gitignore` is missing or misspelled: `cat .gitignore` must show `.venv/`. |
| Prompt has no `(...)` after `source` | Wrong folder or path: `pwd`; Windows Git Bash uses `.venv/Scripts/activate`. |
| `fatal: not a git repository` | Wrong folder: `cd ~/ironhack/DSML-IH-cohort-october26`. |
| `Author identity unknown` | Run the `git config --global user.…` commands from Step 0. |
| `remote origin already exists` | `git remote set-url origin https://github.com/YOUR-USERNAME/DSML-IH-cohort-october26.git` |
| `Support for password authentication was removed` | `gh auth login` (browser), then push again. |
| `rejected … (fetch first)` | GitHub has commits you don't have: `git pull`, then `git push`. |
| `git pull` says *divergent branches* | `git config --global pull.rebase false`, then `git pull`. |
| Vim opened after `git pull` / `commit` | **Esc**, type `:wq`, Enter. |
| `git log` doesn't return | Press `q`. |
| `seq` not found (PowerShell) | Use **Git Bash**, or in PowerShell: `1..24 \| % { mkdir ("week_{0:D2}" -f $_) }` |
