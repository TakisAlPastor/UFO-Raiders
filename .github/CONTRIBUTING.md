# Contributing Guide and Workflow

To maintain the project's architectural integrity and prevent file corruption in Godot, the entire team must strictly adhere to the following operational protocol.

## 1. Fundamental Architecture Rules
- **`main` Protection:** Direct pushes to the `main` branch are strictly forbidden. All code must enter exclusively through an audited Pull Request (PR).
- **Functional Isolation:** All code must run smoothly by pressing `F6` (run current scene) without crashing or throwing yellow/red errors in the Godot console before being sent for review.
- **Coupling (Signal Up, Call Down):** The use of `get_parent()` is completely prohibited. Upward communication in the node tree must be handled exclusively via signals.
- **Static Typing:** Strict typing in GDScript is mandatory (e.g., `var health := 100` or `var speed: float = 50.0`).

---

## 2. Daily Workflow (Git Flow)

Every time you pull a ticket from the "Ready" column on the Kanban board, execute this mechanical process:

### Environment Preparation
Never branch out from an outdated environment.
```bash
git switch main
git pull origin main

```

### Branch Creation

Create a descriptive branch using the appropriate prefix (`feat/` for features, `fix/` for bugs, `refactor/` for cleanup).

```bash
git swithc -c feat/your-task-name

```

### Development and Atomic Commits

Work in isolation. Do not modify scenes unrelated to your task. Save your progress using logical commits.

```bash
git add .
git commit -m "Adds mathematical logic for the health system"

```

### Remote Synchronization

Back up your code frequently to github.

```bash
git push -u origin feat/your-task-name

```

After your first push you don't need to add '-u'

## 3. GitHub Projects
We do not move cards manually from "Ready" to "In Progress". We rely on the automation engine.

1. Upon your first `push` (even if the task is unfinished), go to GitHub and open a **Draft Pull Request**.
2. In the PR description box, you must strictly write the keyword followed by your Issue number. Example:
`Resolves: #15`
*(This will automatically move the Kanban card to "In Progress").*

---

## 4. Code Delivery and Review

When you consider your code ready to be merged into the main game:

1. **Mandatory Synchronization:** Pull changes from main into your local environment.
```bash
git pull origin main

```


*If logical conflicts arise, resolve them locally.*
2. **Self-Audit:** Check 100% of the boxes in your PR template on GitHub.
3. Click the **Ready for Review** button. This flags the code for the Lead Programmer to audit.

---

## 5. Post-Merge Cleanup

Once the Lead Programmer approves and merges your PR, your branch dies. You must clean your local machine to avoid building upon deprecated code.

Execute these commands in strict order:

```bash
git switch main
git pull origin main
git branch -D feat/your-task-name
git fetch -p

```

---

## Scene Conflicts (.tscn / .tres)

If running `git pull origin main` triggers a Merge Conflict inside a `.tscn` or `.tres` file, the file is temporarily broken at the serialization level.

**Golden Rule:** NEVER attempt to open the broken file in the Godot editor, if you did, close it and DON'T save changes. NEVER try to manually fix Git's text arrows (`<<<<<<< HEAD`) in a text editor. You will corrupt the scene permanently.

### How to resolve it:

1. **Abort:** Freeze the environment and cancel the broken merge.
```bash
git merge --abort

```


2. **Consult:** Notify on Discord to determine which version of the scene takes architectural priority (yours or the one in `main`).
3. **Execute the Resolution (Pull again to trigger the conflict and choose ONE option):**

*If the `main` version is more important (Overwrites your local work):*
```bash
git checkout --theirs path/to/scene.tscn

```


*If your local version is more important (Ignores `main`):*
```bash
git checkout --ours path/to/scene.tscn

```


4. **Seal the Resolution:**
```bash
git add path/to/scene.tscn
git commit -m "Commit message"

```


5. Only after this point is it safe to reopen Godot.