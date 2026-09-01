# Lab 01: Git and uv readiness

## Objective

Set up and use a reproducible Python project in a Git repository. By the end of this lab, you will have verified Git and uv, created a uv-managed environment, added project dependencies, run a notebook from that environment, and committed and pushed your work.

The lab is intentionally short! The important outcome is a working Git-and-uv workflow that you will use throughout the course.

## Before you begin

You need all of the following:

- Git and a GitHub account;
- [uv](https://docs.astral.sh/uv/getting-started/installation/); and
- a notebook-capable editor, such as VS Code with its Python and Jupyter extensions.

In a terminal, verify that Git and uv are available:

```bash
git --version
uv --version
```

If either command is not recognized, install the missing program ([Git downloads](https://git-scm.com/downloads) or the [uv installation guide](https://docs.astral.sh/uv/getting-started/installation/)), then close and reopen your terminal before trying again.

If Git later says that it does not know your identity when you commit, configure it once with your own name and email address:

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

## Set up your submission repository

### 1. Create your private submission repository

On [`STAT-486-Fall-2026/lab-01`](https://github.com/STAT-486-Fall-2026/lab-01), select **Use this template** and then **Create a new repository**. Set the owner to the `STAT-486-Fall-2026-Labs` organization, name the repository:

```text
lab-01-<netid>
```

For example, a student whose NetID is `jdoe42` must create `lab-01-jdoe42`. Use your institutional NetID even if it differs from your GitHub username. This convention will be used throughout the course.

Make the repository **private**, then create it. Do not fork the starter repository.

### 2. Clone your repository and inspect it

Copy the HTTPS URL for **your repository**, then run the following commands in a terminal (one at a time might be easier to see what each command does!). Replace the NetID placeholder.

```bash
git clone https://github.com/STAT-486-Fall-2026-Labs/lab-01-<netid>.git
cd lab-01-<netid>
git remote -v
git status
```

`origin` should point to your repository in the `STAT-486-Fall-2026-Labs` organization, and `git status` should report a clean working tree.

> **Do not run `git init`.** Cloning your repository has already created the Git repository. In this lab you will practice navigating, inspecting, committing, and pushing that repository.

### 3. Create and inspect a uv project

This starter deliberately does **not** include `pyproject.toml`, `.python-version`, `uv.lock`, or a virtual environment. Create those project files yourself from the repository root:

```bash
uv init --bare --vcs none --name stat-486-lab-01 --python 3.11
uv python pin 3.11
```

`--bare` makes a minimal project containing just project metadata, which is appropriate for this notebook-focused lab. `--vcs none` prevents uv from initiallizing Git. Git is already present because you cloned the repository, so uv should not try to initialize another repository.

Before creating the environment, use your editor to create a file named `.gitignore` in the repository root with these lines:

```text
.venv/
__pycache__/
.ipynb_checkpoints/
```

As a refresher, the `.gitignore` files tells `git` to `ignore` these files (git it?) :) 
Now add the packages needed for the notebook. Use `uv add`, not `pip install` or `uv pip install`, so that the dependencies are recorded in the project files.

```bash
uv add numpy pandas matplotlib
uv add --dev ipykernel
uv sync
uv tree
uv run python -c "import numpy, pandas, matplotlib; print('Environment is ready')"
```

The `uv add` commands create and synchronize the local `.venv`, update `pyproject.toml`, and create `uv.lock`. `uv sync` is an explicit check that the environment can be rebuilt from those project files, and `uv tree` lets you inspect the installed dependency tree.

Inspect the result before staging anything:

```bash
git status
```

You should see `pyproject.toml`, `.python-version`, `.gitignore`, and `uv.lock`. You should **not** see `.venv/`, because it is intentionally ignored.

### 4. Make the environment commit

Commit the reproducible project setup before doing the notebook work:

```bash
git add pyproject.toml .python-version .gitignore uv.lock
git commit -m "Set up Lab 01 environment"
git push
```

This is a useful Git habit: inspect the changes, stage only the intended files, make a focused commit, and push it to your remote repository.

### 5. Complete the notebook using the project environment

Open `lab-01.ipynb` in your preferred notebook editor. Configure the notebook to use the Python interpreter or kernel from this repository's `.venv`:

- Windows: `.venv\Scripts\python.exe`
- macOS/Linux: `.venv/bin/python`

Do **not** use a global Python interpreter. Run every cell in order and complete each `# your code here` cell and the short written response. The notebook uses NumPy, pandas, and matplotlib; successful imports and a printed interpreter path confirm that the notebook is using your uv environment.

When it is complete, use **Restart Kernel and Run All** (or your editor's equivalent), save the notebook, and make a second focused commit:

```bash
git status
git add lab-01.ipynb
git commit -m "Complete Lab 01"
git push
git status
```

The final `git status` should report a clean working tree. Never commit `.venv/`.

## Completion checklist

Before submitting, confirm all of the following:

- `git remote -v` shows your repository in `STAT-486-Fall-2026-Labs` as `origin`.
- `pyproject.toml` lists `numpy`, `pandas`, and `matplotlib`; `ipykernel` is recorded as a development dependency.
- `uv.lock` is committed, while `.venv/` is not.
- `lab-01.ipynb` runs from top to bottom with the repository `.venv` kernel.
- `git log --oneline -2` shows your environment and notebook commits.
- `git status` is clean after your final push.

## Submission

Paste the root URL of your repository into the Canvas Lab 01 submission textbox:

```text
https://github.com/STAT-486-Fall-2026-Labs/lab-01-<netid>
```

Submit the repository URL, not the course starter URL, a notebook-view URL, a clone URL ending in `.git`, or a local file path. Do not open a pull request and do not upload the notebook file to Canvas.
