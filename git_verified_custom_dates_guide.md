# Git Verified Badge with Custom Dates Guide

## Why "Verified" Badge Disappears

When you change commit dates using environment variables or `--amend`, it can break GPG signatures, causing the "Verified" badge to disappear on GitHub.

## Solution 1: GPG Sign Commits with Custom Dates

### Step 1: Install and Setup GPG

**On macOS:**
```bash
# Install GPG
brew install gnupg

# Generate GPG key
gpg --full-generate-key
```

**On Linux:**
```bash
# Install GPG (Ubuntu/Debian)
sudo apt-get install gnupg

# Generate GPG key
gpg --full-generate-key
```

### Step 2: Configure Git with GPG

```bash
# List your GPG keys
gpg --list-secret-keys --keyid-format=long

# Configure Git with your GPG key ID
git config --global user.signingkey YOUR_GPG_KEY_ID
git config --global commit.gpgsign true

# Add GPG key to GitHub (copy the public key)
gpg --armor --export YOUR_GPG_KEY_ID
```

### Step 3: Sign Commits with Custom Dates

```bash
# Add the -S flag to sign commits
GIT_AUTHOR_DATE="2022-05-01T12:00:00" GIT_COMMITTER_DATE="2022-05-01T12:00:00" git commit -S -m "Verified commit with custom date"

# Push to GitHub
git push
```

## Solution 2: Sign Existing Commits (Retroactive)

### Method A: Amend Last Commit
```bash
# Sign the last commit with custom date
git commit --amend --date="2022-05-01T12:00:00" -S --no-edit

# Force push to update GitHub
git push --force
```

### Method B: Interactive Rebase (Multiple Commits)
```bash
# Start interactive rebase
git rebase -i HEAD~3

# In editor, change 'pick' to 'edit' for commits to sign
# For each commit:
git commit --amend -S --no-edit
git rebase --continue

# Force push
git push --force
```

## Solution 3: SSH Signing (Alternative to GPG)

GitHub also supports SSH commit signing:

```bash
# Configure SSH signing
git config --global gpg.format ssh
git config --global user.signingkey ~/.ssh/id_ed25519.pub
git config --global commit.gpgsign true

# Sign with SSH and custom date
GIT_AUTHOR_DATE="2022-05-01T12:00:00" GIT_COMMITTER_DATE="2022-05-01T12:00:00" git commit -S -m "SSH signed commit"
```

## Complete Example: Custom Date + Verified Badge

```bash
# 1. Setup (one time)
brew install gnupg  # or your system's package manager
gpg --full-generate-key
git config --global user.signingkey YOUR_GPG_KEY_ID
git config --global commit.gpgsign true

# 2. Add your public key to GitHub
gpg --armor --export YOUR_GPG_KEY_ID
# Copy output and add to GitHub → Settings → SSH and GPG keys

# 3. Create verified commit with custom date
echo "# Verified custom date commit" > verified_test.md
git add verified_test.md
GIT_AUTHOR_DATE="2022-01-01T00:00:00" GIT_COMMITTER_DATE="2022-01-01T00:00:00" git commit -S -m "Happy New Year 2022 - Verified!"
git push
```

## Troubleshooting

### GPG Agent Issues
```bash
# Restart GPG agent
gpgconf --kill gpg-agent
gpgconf --launch gpg-agent

# Test GPG signing
echo "test" | gpg --clearsign
```

### Automatic Signing
```bash
# Enable automatic signing for all commits
git config --global commit.gpgsign true

# Now all commits will be signed automatically
GIT_AUTHOR_DATE="2022-01-01T00:00:00" GIT_COMMITTER_DATE="2022-01-01T00:00:00" git commit -m "Auto-signed with custom date"
```

## Key Points

1. **GPG signing preserves the "Verified" badge** even with custom dates
2. **The `-S` flag** is required for manual signing
3. **Both author and committer dates** can be custom while maintaining verification
4. **GitHub shows verification** based on GPG signature, not commit timestamp
5. **SSH signing** is a simpler alternative to GPG

## Verification Commands

```bash
# Check if commit is signed
git log --show-signature -1

# Verify GPG signature
git verify-commit HEAD