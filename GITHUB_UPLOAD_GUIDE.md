# How to Upload DeployNotes to GitHub
## Step-by-Step Visual Guide

---

## What You'll Need

- A GitHub account (free at github.com)
- The `deploynotes-repo.zip` file (download below)
- About 5 minutes

---

## Step 1: Create a GitHub Account (Skip if You Have One)

1. Go to **[github.com](https://github.com)**
2. Click **"Sign up"**
3. Enter your email, create password, choose username
4. Verify your email

---

## Step 2: Create a New Repository

1. **Log in** to GitHub
2. Click the **"+"** icon in the top right corner
3. Select **"New repository"**

![New Repo Button Location: Top right corner, click + then "New repository"]

---

## Step 3: Configure Your Repository

Fill in the form:

| Field | What to Enter |
|-------|---------------|
| **Repository name** | `deploynotes` (lowercase, no spaces) |
| **Description** | `Secure offline field reporting app for humanitarian deployments` |
| **Visibility** | ✅ Select **Public** (required for free GitHub Pages) |
| **Initialize** | ❌ Leave all checkboxes UNCHECKED |

Then click the green **"Create repository"** button.

```
┌─────────────────────────────────────────────────────────┐
│  Create a new repository                                │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  Repository name:  [ deploynotes          ]             │
│                                                         │
│  Description:      [ Secure offline field reporting... ]│
│                                                         │
│  ○ Public   ← SELECT THIS                               │
│  ○ Private                                              │
│                                                         │
│  □ Add a README file        ← LEAVE UNCHECKED           │
│  □ Add .gitignore           ← LEAVE UNCHECKED           │
│  □ Choose a license         ← LEAVE UNCHECKED           │
│                                                         │
│            [ Create repository ]                        │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## Step 4: Upload Your Files

After creating the repo, you'll see a setup page. Look for the link that says:

**"uploading an existing file"**

Click it.

```
Quick setup — if you've done this kind of thing before

    …or create a new repository on the command line
    …or push an existing repository from the command line
    …or import code from another repository

───────────────────────────────────────────────────────────
    
    Get started by creating a new file or uploading an existing file
                                         ^^^^^^^^^^^^^^^^^^^^^^^^
                                         CLICK THIS LINK
```

---

## Step 5: Drag and Drop Files

1. **Unzip** the `deploynotes-repo.zip` file on your computer
2. **Open** the `deploynotes-repo` folder
3. **Select ALL files** inside (including the hidden `.gitignore`):
   - `.gitignore`
   - `index.html`
   - `LICENSE`
   - `README.md`
   - `DeployNotes_QuickStart.md`
   - `DeployNotes_Technical_Overview.md`
   - `DeployNotes_User_Guide.md`

4. **Drag and drop** them onto the GitHub upload area

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│     Drag files here to add them to your repository      │
│                                                         │
│              or choose your files                       │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │  📄 index.html                              ✓   │   │
│  │  📄 README.md                               ✓   │   │
│  │  📄 LICENSE                                 ✓   │   │
│  │  📄 DeployNotes_User_Guide.md               ✓   │   │
│  │  📄 DeployNotes_QuickStart.md               ✓   │   │
│  │  📄 DeployNotes_Technical_Overview.md       ✓   │   │
│  │  📄 .gitignore                              ✓   │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

**Tip for hidden files:** On Mac, press `Cmd + Shift + .` to show hidden files. On Windows, check "Hidden items" in the View menu.

---

## Step 6: Commit Your Files

1. Scroll down to the **"Commit changes"** section
2. In the commit message box, type: `Initial commit - DeployNotes v1.0`
3. Keep **"Commit directly to the main branch"** selected
4. Click the green **"Commit changes"** button

```
┌─────────────────────────────────────────────────────────┐
│  Commit changes                                         │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  [ Initial commit - DeployNotes v1.0    ]               │
│                                                         │
│  [ Add an optional extended description...          ]   │
│                                                         │
│  ● Commit directly to the main branch                   │
│  ○ Create a new branch                                  │
│                                                         │
│            [ Commit changes ]                           │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## Step 7: Enable GitHub Pages

Now let's make your app accessible via a public URL.

1. Go to your repository page (you should already be there)
2. Click **"Settings"** (tab near the top, with gear icon)
3. In the left sidebar, click **"Pages"**
4. Under **"Source"**, select:
   - Branch: **main**
   - Folder: **/ (root)**
5. Click **"Save"**

```
┌─────────────────────────────────────────────────────────┐
│  Settings → Pages                                       │
├─────────────────────────────────────────────────────────┤
│                                                         │
│  GitHub Pages                                           │
│                                                         │
│  Source                                                 │
│  ┌─────────────────────────────────────────────────┐   │
│  │  Branch: [ main ▼ ]    Folder: [ / (root) ▼ ]   │   │
│  └─────────────────────────────────────────────────┘   │
│                            [ Save ]                     │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

---

## Step 8: Get Your Live URL

After saving, wait 1-2 minutes, then refresh the page.

You'll see a green box with your live URL:

```
┌─────────────────────────────────────────────────────────┐
│  ✅ Your site is live at                                │
│                                                         │
│     https://yourusername.github.io/deploynotes/         │
│                                                         │
│                    [ Visit site ]                       │
└─────────────────────────────────────────────────────────┘
```

**🎉 That's your app URL!** Share it with your team.

---

## Step 9: Test It

1. Open the URL on your phone
2. You should see the DeployNotes PIN screen
3. Create a PIN
4. Verify everything works
5. Test "Add to Home Screen" installation

---

## Step 10: Share with Your Team

Send this message via WhatsApp or email:

```
📋 DeployNotes is ready!

Install the app:
1. Open this link: https://yourusername.github.io/deploynotes/
2. Tap Share → "Add to Home Screen"
3. Create your PIN
4. You're ready to capture field reports!

Works offline - no internet needed after install.
```

---

## Troubleshooting

### "404 - Page not found"
- Wait 2-3 minutes for GitHub Pages to deploy
- Make sure the file is named `index.html` (not `Index.html` or `deploynotes.html`)
- Check that GitHub Pages is enabled in Settings → Pages

### "Files won't upload"
- Try uploading fewer files at a time
- Make sure you're uploading individual files, not the zip folder
- Check your internet connection

### "Can't see .gitignore file"
- It's a hidden file (starts with a dot)
- Mac: Press `Cmd + Shift + .` to show hidden files
- Windows: View → Check "Hidden items"
- It's optional - the app works without it

### "GitHub Pages shows README instead of app"
- The HTML file must be named `index.html` exactly
- Make sure it's in the root folder, not inside a subfolder

---

## Summary Checklist

- [ ] Created GitHub account
- [ ] Created new repository named `deploynotes`
- [ ] Uploaded all 7 files
- [ ] Committed changes
- [ ] Enabled GitHub Pages (main branch, root folder)
- [ ] Got live URL
- [ ] Tested on phone
- [ ] Shared with team

---

## Your Final URLs

| URL | What It Shows |
|-----|---------------|
| `github.com/yourusername/deploynotes` | Your repository (code & docs) |
| `yourusername.github.io/deploynotes` | Your live app (share this!) |

---

**Need help?** Open an issue in your GitHub repository or contact your tech support.
