# github-sandbox

# 📥 Download Files via Commit Message

A GitHub Actions workflow that lets you download files into your repository just by writing a special commit message — no terminal or command line needed.

---

## ⚙️ Setup

0. Fork this repo
1. Go to your repository on GitHub
2. Click **Settings** → **Actions** → **General**
3. Scroll down to **Workflow permissions**
4. Select **Read and write permissions** and click **Save**

That's it — no tokens or secrets needed.

### Supported Platforms

- **YouTube** (fully tested) — also supports YouTube Music
- **Bunkr** (fully tested)
- Other streaming sites may work with yt-dlp
- **any publickly available downloadable link**

---

## 🔐 YouTube Cookies (Optional, for age-restricted/paid content)

If downloading age-restricted or member-only YouTube videos, you'll need to add cookies:

### Step 1: Get cookies.txt

1. Install **"Get cookies.txt LOCALLY"** extension in Chrome
2. Enable **Allow in incognito**
3. Open an **Incognito window** → log into YouTube
4. Visit `https://www.youtube.com/robots.txt`
5. Right-click extension icon → **"Export"**
6. Save as `youtube_cookies.txt`
7. Close incognito, never use it again

### Step 2: Add to GitHub Secrets

1. Go to your repository on GitHub
2. Click **Settings** → **Secrets and variables** → **Actions**
3. Click **New repository secret**
4. Name: `YOUTUBE_COOKIES`
5. Open your `youtube_cookies.txt` file, copy everything
6. Paste into the **Secret** field
7. Click **Add secret**

That's it — the workflow will use these cookies automatically.

### ⚠️ Cookies Expire

YouTube rotates cookies frequently (typical lifespan 3-5 days) due to YouTube's anti-bot measures.
You may need to re-export cookies periodically.
If downloads start failing, fetch new cookies and update the secret.
**YouTube detects bot most of the time; cookies required almost always.**

---

### How to trigger a download

1. Open any file in your repository on GitHub (for example, this `README.md`)
2. Click the **pencil icon** (✏️) at the top right to edit it
3. Make any small change (add a space, a blank line, anything)
4. Scroll down to the **Commit changes** section
5. Select **Commit directly to the `main` branch**
6. In the commit message box, type one of the commands below
7. Click **Commit changes**

The workflow will run automatically and the downloaded files will appear in the `downloads/` folder.

### ⚡ Quick Commands (No Editor Needed)

Trigger downloads without editing any file:
### Download
```bash
git commit --allow-empty -m "download: https://example.com/file.zip" && git push
```
### Cancel/Clean (undo last commit)
```bash
git reset --hard HEAD~1 && git push --force
```

---

## 📝 Commands

### Download files individually

Downloads each file and saves it by its original filename.

```
download: URL1 URL2 URL3
```

**Examples:**

```
download: https://example.com/file.zip
```

```
download: https://example.com/a.zip https://example.com/b.pdf https://example.com/c.zip
```

---

### Download and archive into a single ZIP

Downloads all files and bundles them into one timestamped `.zip` archive saved to `downloads/`.

```
download-zip: URL1 URL2 URL3
```

**Examples:**

```
download-zip: https://example.com/file.zip
```

```
download-zip: https://example.com/a.zip https://example.com/b.pdf https://example.com/c.zip
```

The resulting archive will be named like: `archive_20250423_153012.zip`

---

## 📁 Output

| Command | Result |
|---|---|
| `download:` | Each file saved individually in `downloads/` with its original name |
| `download-zip:` | All files bundled into a single `archive_YYYYMMDD_HHMMSS.zip` in `downloads/` |

---

## ⬇️ Extracting split files

If your download was split into multiple `.part_N` files (common with large files), reassemble them:

### Linux / macOS

```bash
cat downloads/filename.part_* > downloads/original_filename
```

### Windows (Command Prompt)

```cmd
copy /b downloads\filename.part_* downloads\original_filename
```

### Windows (GUI)

Use **7-Zip** → right-click the first `.part_1` file → "7-Zip" → "Combine files..." → save as original filename.

---

## 👀 Checking the result

After committing, you can monitor the workflow:

1. Click the **Actions** tab in your repository
2. Click the latest workflow run to see progress and logs
3. Once it completes, go back to the **Code** tab and open the `downloads/` folder to find your files

---

## Notes

- URLs must be publicly accessible (no login required)
- Separate multiple URLs with spaces
- The workflow skips itself using `[skip ci]` in its own commit message to avoid infinite loops
- If no valid `download:` or `download-zip:` command is found in the commit message, the workflow will exit without doing anything
