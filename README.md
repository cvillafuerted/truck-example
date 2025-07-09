# 🧼 Git Rebase Workflow Guide (Trunk-Based Development)

A practical and professional guide to keep your branches clean using `git rebase -i`. Ideal for Trunk-Based Development and clean Pull Request practices.

---

## 📋 Goal

Keep your feature branches clean and linear before merging them into `develop` or `main`, allowing better readability, simpler code reviews, and cleaner commit history.

---

## 🛠️ Step-by-Step Workflow

---

### ✅ 1. During Development (Clean up progressively)

As you make commits:

```bash
git rebase -i HEAD~N
```

Replace `N` with how many recent commits you want to clean up.

- **Use `pick`** to keep a commit as is.
- **Use `squash`** to combine it with the one above and allow editing the message.
- **Use `fixup`** to combine with the one above and discard the message.
- **Use `reword`** to change a commit message.
- **Use `drop`** to delete a commit.

Example:

```text
fixup   123abc Initial form
fixup  456def Fix spacing
reword 789ghi Add validation
```

Then save and close the editor.

---

### ✅ 2. Regularly update `develop` locally

```bash
git fetch origin
```

This ensures your local `origin/develop` is up to date with the remote.

---

### ✅ 3. Final Rebase Before Merging or Creating a Pull Request

Once your feature is ready:

```bash
git rebase -i origin/develop
```

- This moves your commits on top of the latest `develop`.
- Combine commits using `squash` or `fixup` to prepare a clean, logical history.
- Ideal before creating a PR or merging.

---

### ⚠️ 4. Handling Conflicts

If Git finds conflicts during the rebase:

1. VS Code (or your editor) will show conflicted files.
2. Manually fix them.
3. Stage the fixed files:
   ```bash
   git add .
   ```
4. Continue the rebase:
   ```bash
   git rebase --continue
   ```
5. Repeat if there are more conflicts.

If you want to cancel the rebase:

```bash
git rebase --abort
```

---

### ✅ 5. Push Changes After Rebase

#### If you **haven’t pushed yet**:

```bash
git push origin feature/my-feature
```

#### If you **already pushed earlier**, you must force-push:

```bash
git push --force-with-lease
```

> 🔐 Use `--force-with-lease`, not just `--force`, to avoid overwriting other people’s work accidentally.

---

### 🔁 6. Made More Commits Later?

No problem — just repeat the cycle:

1. Clean recent commits (if needed):

   ```bash
   git rebase -i HEAD~N
   ```

2. Rebase again on `develop`:

   ```bash
   git fetch origin
   git rebase -i origin/develop
   ```

3. Push again:
   ```bash
   git push --force-with-lease
   ```

---

## 💡 Best Practices

| Practice                           | Reason                                     |
| ---------------------------------- | ------------------------------------------ |
| Fetch before rebase                | Ensures rebasing on the latest `develop`   |
| Use `fixup` for small changes      | Keeps history clean                        |
| Test your app after a rebase       | Avoids errors from broken commit sequences |
| Avoid `merge develop` into feature | Prefer rebase to keep linear history       |
| Use `--force-with-lease`           | Safer push, prevents accidental overwrites |

---

## 📦 Visual Example

### Before:

```
develop:     A---B
               \
feature:        C---D---E
```

### After `rebase -i origin/develop`:

```
develop:     A---B
                   \
feature:            C'
```

---

## 🧼 Summary

| Step                             | Command                               |
| -------------------------------- | ------------------------------------- |
| Clean recent commits             | `git rebase -i HEAD~3`                |
| Update base branch               | `git fetch origin`                    |
| Final cleanup and sync with base | `git rebase -i origin/develop`        |
| Resolve conflicts (if any)       | `git add .` → `git rebase --continue` |
| Push safely                      | `git push --force-with-lease`         |
| Repeat if new commits are added  | Clean again → Rebase → Push again     |

---

Rebase responsibly — your teammates and future self will thank you! 💻✨
