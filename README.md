# Student Portfolio Template (GitHub Pages)

This repo is a **minimal, free** portfolio site powered by **GitHub Pages** using the default *minima* theme.

## How to use
1. Click **Use this template** → **Create a new repository** on your GitHub account.
2. Name it `2025-atc-portfolio-<your-name>` (or any name you like).
3. Add your name to `_config.yml` (title).
4. Push a commit.
5. In GitHub → **Settings** → **Pages** → **Build and deployment** → **Source**: select **Deploy from a branch**.
6. Choose **main** and `/ (root)` and save. Your site will publish at `https://<your-username>.github.io/<repo>/`.

## Structure
```
/about/     # bio + goals
/standards/ # course one-pagers + checklists mapped to standards
/projects/  # client/server demo, utilities, mini-apps
/kb/        # help desk knowledge base articles
/logs/      # device clinic logs: drivers/firmware/backup notes
/net/       # subnetting sheet, ports table, OSI/TCP/IP diagram
/scripts/   # tiny automation utilities (e.g., export osTicket stats)
```
Edit and add pages as Markdown (`.md`). Each page should end with **Alignment** tags like:

```
**Alignment:** [Course: Internet Technologies 110917] [Bullets: 7,9,10]
```

## Safety & privacy
- Never include real PHI/PII. Use demo/de-identified data only.
- Ask your instructor before posting screenshots with names/IDs/IPs.
