# ðŸ’™ Git-FTP Made Easy â€” Complete Beginner & Advanced Guide

**Prepared by:** Shaik Sameer Shubhan  
**Theme:** Professional Blue Edition  
**Version:** 3.0 (Includes Windows Installation + Automation + Examples)

---

## ðŸ§­ Overview

**Git-FTP** is a Git extension that lets you **deploy your project directly to an FTP server** using Git commands.  
It uploads **only changed files** since the last commit â€” saving time, bandwidth, and effort.

---

## âš™ï¸ Installation on Windows

### ðŸ§ Method 1: Git Bash (Recommended)

1. Open **Git Bash**.  
2. Run:
   ```bash
   curl -O https://raw.githubusercontent.com/git-ftp/git-ftp/master/git-ftp
   chmod +x git-ftp
   sudo mv git-ftp /usr/local/bin/
   ```
3. Verify installation:
   ```bash
   git ftp --version
   ```

âœ… *If version appears â€” installation succeeded.*

---

### ðŸ« Method 2: Chocolatey (Simplest)
```bash
choco install git-ftp
```

Verify:
```bash
git ftp --version
```

---

## ðŸš€ Basic Git-FTP Commands (with Examples)

### 1ï¸âƒ£ `git ftp init` â€” First-time upload
Uploads the entire project to FTP for the first time.

```bash
git ftp init -u username -p password ftp://example.com/public_html/
```
ðŸ“˜ *Use only once to initialize deployment.*

```mermaid
flowchart LR
A[Local Repo] -->|init upload all| B[FTP Server]
B --> C[Live Website]
```

---

### 2ï¸âƒ£ `git ftp push` â€” Upload only changes
Uploads only modified files since last deployment.

```bash
git ftp push -u username -p password ftp://example.com/public_html/
```
ðŸ“˜ *Fast sync of updated files only.*

```mermaid
flowchart LR
A[Local Git Repo] -->|push changed files| B[FTP Server]
```

---

### 3ï¸âƒ£ `git ftp catchup` â€” Mark server as up to date
When you already have files on FTP but not deployed with Git-FTP.

```bash
git ftp catchup -u username -p password ftp://example.com/public_html/
```
ðŸ“˜ *Syncs commit hash to server without uploading anything.*

```mermaid
flowchart TD
A[FTP Server Existing Files] --> B[Catchup Command]
B --> C[Git Repo Hash Synced]
```

---

### 4ï¸âƒ£ `git ftp show` â€” View last deployed commit
```bash
git ftp show
```
ðŸ“˜ *Displays last uploaded Git commit hash.*

```mermaid
flowchart LR
A[FTP Metadata File] -->|show| B[Commit Hash Output]
```

---

### 5ï¸âƒ£ `git ftp log` â€” View upload logs
```bash
git ftp log
```
ðŸ“˜ *Shows deployment history and timestamps.*

```mermaid
flowchart TD
A[FTP Server Log File] -->|log| B[Developer Console Output]
```

---

### 6ï¸âƒ£ `git ftp clean` â€” Remove deleted files
```bash
git ftp clean
```
ðŸ“˜ *Deletes remote files removed locally.*

```mermaid
flowchart LR
A[Local Files Removed] -->|clean| B[FTP Server Removes Same Files]
```

---

### 7ï¸âƒ£ `git ftp download` â€” Download remote files
```bash
git ftp download ftp://example.com/public_html/
```
ðŸ“˜ *Pulls project files from FTP to local folder.*

```mermaid
flowchart LR
A[FTP Server Files] -->|download| B[Local Machine]
```

---

## ðŸ§© Configuration Commands

Before using Git-FTP, configure once per project:

```bash
git config git-ftp.url "ftp://example.com/public_html/"
git config git-ftp.user "username"
git config git-ftp.password "password"
git config git-ftp.syncroot "dist/"
git config git-ftp.insecure 1
```

âœ… *These are stored inside `.git/config` for auto-use.*

```mermaid
flowchart TD
A[git config commands] --> B[.git/config file]
B --> C[Used by future git ftp commands]
```

---

## ðŸ“ Include & Ignore Files

Control what gets uploaded:

### `.git-ftp-ignore`
```
node_modules/
README.md
.gitignore
```

### `.git-ftp-include`
```
!dist/*
!index.html
```

```mermaid
flowchart LR
A[Project Files] -->|filter| B[Ignore Rules Applied]
B -->|upload| C[FTP Server Final Files]
```

---

## âš™ï¸ Advanced Commands & Flags

| Command | Description | Example |
|----------|--------------|----------|
| `git ftp push --auto-init` | Auto initializes if FTP not yet set up | `git ftp push --auto-init ftp://example.com/public_html/` |
| `git ftp push --dry-run` | Show which files would be uploaded | `git ftp push --dry-run` |
| `git ftp push --force` | Force upload even if hash mismatch | `git ftp push --force` |
| `git ftp push -v` | Verbose output for debugging | `git ftp push -v` |
| `git ftp push --syncroot build/` | Upload only a folder | `git ftp push --syncroot build/` |

```mermaid
flowchart TD
A[Advanced Flags] -->|modify behavior| B[git ftp push process]
B -->|upload| C[FTP Server]
```

---

## ðŸ§  Examples for Common Workflows

### ðŸ”¹ Full Deployment (First Time)
```bash
git ftp init -u user -p pass ftp://example.com/public_html/
```

### ðŸ”¹ Normal Update
```bash
git add .
git commit -m "Updated homepage"
git ftp push
```

### ðŸ”¹ After Manual FTP Upload
```bash
git ftp catchup
git ftp push
```

---

## â˜ï¸ CI/CD Automation â€” GitHub Actions

Automate deployment when pushing to `main` branch.

```yaml
name: Deploy Website via Git-FTP
on:
  push:
    branches: [ main ]

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Install Git-FTP
        run: sudo apt-get install git-ftp -y

      - name: Deploy via Git-FTP
        run: |
          git ftp push --auto-init             --user ${{ secrets.FTP_USER }}             --passwd ${{ secrets.FTP_PASS }}             ftp://ftp.example.com/public_html/
```

```mermaid
flowchart TD
A[Developer Pushes Code] --> B[GitHub Actions Workflow]
B --> C[Install Git-FTP]
C --> D[Run git ftp push]
D --> E[FTP Server Updated Automatically]
E --> F[Website Live!]
```

---

## ðŸ’¡ Tips & Best Practices

- Use **environment variables** for credentials:
  ```bash
  export GIT_FTP_USER="username"
  export GIT_FTP_PASSWD="password"
  ```
- Always run `git ftp catchup` before first push to existing server.  
- Avoid saving plain passwords â€” use GitHub Secrets or `.netrc`.  
- Maintain `.git-ftp-ignore` to skip unnecessary files.

---

## ðŸ§° Troubleshooting

| Issue | Cause | Fix |
|-------|--------|-----|
| `Authentication failed` | Wrong credentials | Check username/password |
| `Permission denied` | FTP user lacks write access | Update FTP permissions |
| `Unknown option` | Outdated version | Update Git-FTP |
| `No files uploaded` | Hash mismatch | Run `git ftp catchup` then `git ftp push` |

---

## ðŸ§¾ Command Summary

| Command | Purpose | Example |
|----------|----------|----------|
| `git ftp init` | Upload everything (first time) | `git ftp init ftp://...` |
| `git ftp push` | Upload changed files | `git ftp push ftp://...` |
| `git ftp catchup` | Sync server with current commit | `git ftp catchup ftp://...` |
| `git ftp show` | Show deployed commit hash | `git ftp show` |
| `git ftp log` | Show deployment history | `git ftp log` |
| `git ftp clean` | Remove deleted files from FTP | `git ftp clean` |
| `git ftp download` | Download remote files | `git ftp download ftp://...` |
| `git ftp push --dry-run` | Preview uploads | `git ftp push --dry-run` |
| `git ftp push --force` | Force upload | `git ftp push --force` |
| `git ftp push --auto-init` | Initialize automatically | `git ftp push --auto-init ftp://...` |

---

## ðŸŒ References

- ðŸ”— [Git-FTP GitHub Repo](https://github.com/git-ftp/git-ftp)  
- ðŸ”— [Git-SCM Documentation](https://git-scm.com/docs/git-ftp)  
- ðŸ”— [DigitalOcean FTP Deployment Guide](https://www.digitalocean.com/community/tutorials/how-to-use-git-ftp-to-deploy-your-website)

---

## ðŸ Summary

**Git-FTP** streamlines your website deployment process â€” just push your Git commits, and your site stays perfectly synced with your repo.  
Itâ€™s lightweight, fast, and ideal for simple web hosting servers.

ðŸ’™ *Now you can deploy smarter, not harder.*
