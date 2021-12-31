# Git Delete Commits Guide

## Method 1: Delete Last Commit (Keep Changes) - SAFEST

```bash
# Remove last commit but keep the changes staged
git reset --soft HEAD~1

# To delete from GitHub too (REQUIRED after reset)
git push --force
```

## Method 2: Delete Last Commit (Remove Changes) - DESTRUCTIVE

```bash
# Remove last commit AND all changes permanently
git reset --hard HEAD~1

# To delete from GitHub too
git push --force
```

## Method 3: Delete Multiple Commits

```bash
# Delete last 3 commits (keep changes)
git reset --soft HEAD~3

# Delete last 3 commits (remove changes)
git reset --hard HEAD~3

# Force push to update GitHub
git push --force
```

## Method 4: Delete Specific Commit (Interactive Rebase)

```bash
# Start interactive rebase for last 5 commits
git rebase -i HEAD~5

# In the editor, delete the line with the commit you want to remove
# Save and exit

# Force push to GitHub
git push --force
```

## Method 5: Reset to Specific Commit

```bash
# Find the commit hash you want to keep
git log --oneline

# Reset to specific commit (example: a56888c)
git reset --hard a56888c

# Force push
git push --force
```

## ⚠️ IMPORTANT WARNINGS

### Force Push Requirements
- **ALWAYS** use `git push --force` after deleting commits
- This overwrites GitHub history to match your local changes
- Without force push, GitHub will still show the deleted commits

### Collaboration Risks
- **NEVER** force push on shared branches where others are working
- Use `git push --force-with-lease` for safer force pushing
- Inform team members before rewriting shared history

### Alternative: Revert Instead of Delete
```bash
# Creates a new commit that undoes changes (safer for shared repos)
git revert HEAD
git push  # No force needed
```

## Recovery Options

### Recover Deleted Commits
```bash
# Find deleted commit hash
git reflog

# Restore deleted commit
git reset --hard <commit-hash>
```

### Safer Force Push
```bash
# Only force push if remote hasn't changed
git push --force-with-lease
```

## Summary Commands

| Action | Command | GitHub Update |
|--------|---------|---------------|
| Delete last commit (keep changes) | `git reset --soft HEAD~1` | `git push --force` |
| Delete last commit (remove changes) | `git reset --hard HEAD~1` | `git push --force` |
| Delete multiple commits | `git reset --soft HEAD~N` | `git push --force` |
| Undo changes safely | `git revert HEAD` | `git push` |

**Remember**: Force push is REQUIRED to delete commits from GitHub after local deletion!