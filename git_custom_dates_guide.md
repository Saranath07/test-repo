# Git Custom Commit Dates Guide

## Method 1: Complete Past Commit (RECOMMENDED for GitHub)

```bash
# Set BOTH author and committer dates to appear completely in the past
GIT_AUTHOR_DATE="2023-01-15T10:30:00" GIT_COMMITTER_DATE="2023-01-15T10:30:00" git commit -m "Complete past commit"

# This makes it appear as if the commit was made entirely on that date
# Both GitHub and git logs will show the historical date
```

## Method 2: Using `--date` flag (Changes Author Date Only)

```bash
# ⚠️ WARNING: This only changes author date, committer date remains current
git commit --date="2023-01-15 10:30:00" -m "Partial past commit"

# Examples
git commit --date="2022-12-25 09:00:00" -m "Christmas commit"
git commit --date="last week" -m "Commit from last week"
git commit --date="3 days ago" -m "Commit from 3 days ago"
```

## Method 3: Using Environment Variables (Most Flexible)

```bash
# Set both author and commit dates
export GIT_AUTHOR_DATE="2023-01-15T10:30:00"
export GIT_COMMITTER_DATE="2023-01-15T10:30:00"
git commit -m "Commit with environment variables"

# Or set them inline (RECOMMENDED)
GIT_AUTHOR_DATE="2023-01-15T10:30:00" GIT_COMMITTER_DATE="2023-01-15T10:30:00" git commit -m "Inline env vars"
```

## Method 3: Amending the Last Commit's Date

```bash
# Change the date of the most recent commit
git commit --amend --date="2023-01-15 10:30:00"

# Change both author and committer date
git commit --amend --date="2023-01-15 10:30:00" --reset-author
```

## Method 4: Using Interactive Rebase (for multiple commits)

```bash
# Start interactive rebase for last 3 commits
git rebase -i HEAD~3

# In the editor, change 'pick' to 'edit' for commits you want to modify
# Then for each commit:
git commit --amend --date="2023-01-15 10:30:00"
git rebase --continue
```

## Method 5: Creating Commits in the Past with Specific Format

```bash
# Using ISO 8601 format
git commit --date="2023-01-15T10:30:00+05:30" -m "ISO format commit"

# Using RFC 2822 format  
git commit --date="Sun, 15 Jan 2023 10:30:00 +0530" -m "RFC format commit"

# Using Unix timestamp
git commit --date="1673766000" -m "Unix timestamp commit"
```

## Important Notes

1. **Author Date vs Committer Date**: 
   - `--date` only changes the author date
   - Use environment variables to change both dates

2. **GitHub Display**: 
   - GitHub shows the author date in the commit history
   - The committer date is visible in detailed views

3. **Time Zones**: 
   - Git respects time zones in dates
   - Use format like `+0530` for Indian Standard Time

4. **Relative Dates**:
   - Git accepts relative dates like "yesterday", "last week", "3 days ago"

## Verification Commands

```bash
# Check commit dates
git log --format="%h %ad %cd %s" --date=iso
git log --format="%h %ad %s" --date=short
git show --format=fuller HEAD