# DeployNotes
## Field Reporting App - Installation & User Guide

---

## Table of Contents
1. [What is DeployNotes?](#what-is-deploynotes)
2. [Download the App](#download-the-app)
3. [Sharing with Your Team](#sharing-with-your-team)
4. [Installing on Your Phone](#installing-on-your-phone)
5. [First-Time Setup](#first-time-setup)
6. [Using the App](#using-the-app)
7. [Syncing & Exporting Data](#syncing--exporting-data)
8. [Settings & Configuration](#settings--configuration)
9. [Troubleshooting](#troubleshooting)

---

## What is DeployNotes?

DeployNotes is a secure, offline-capable field reporting app designed for humanitarian deployments, disaster relief, and field work. 

### Key Features:
- **🔐 PIN Protected** - Secure access to sensitive field data
- **📴 Works Offline** - Captures and stores everything locally, syncs when wifi is available
- **📸 Photo & Video Capture** - Document with media directly in the app
- **☁️ HIPAA-Ready Upload** - Configure your own compliant cloud storage
- **📍 Flexible Location Tracking** - Deployment, Parish, Community, Facility
- **📖 Story Capture** - Document human stories with consent tracking
- **🏥 Health Risk Logging** - Quick checkboxes for common field observations

---

## Download the App

### Step 1: Get the File
Download **DeployNotes.html** from wherever you received it (email, cloud link, etc.)

This single HTML file IS the entire app - no installation from an app store required.

### Step 2: Save It
Save the file somewhere you can access it:
- Your phone's Files app
- Google Drive
- Dropbox
- Your computer

---

## Sharing with Your Team

You have three options for distributing DeployNotes to team members:

### Option A: Direct File Share (Simplest)
1. Send **DeployNotes.html** via WhatsApp, email, or AirDrop
2. Each person saves the file to their phone
3. Open in browser → Works immediately

**Pros:** No hosting needed, works instantly
**Cons:** Each person has a separate copy

---

### Option B: Cloud Storage Link (Easy)
1. Upload **DeployNotes.html** to Google Drive, Dropbox, or OneDrive
2. Set sharing to "Anyone with link can view"
3. Share the link with your team
4. Team members download to their phone

**Pros:** Single source, easy updates
**Cons:** Requires download step

---

### Option C: Web Hosting (Best for Teams)
Host the file online so team members can install it like an app.

#### Free Hosting Options:

**GitHub Pages (Recommended - Free)**
1. Create a free GitHub account at github.com
2. Create a new repository (e.g., "deploynotes")
3. Upload DeployNotes.html
4. Go to Settings → Pages → Enable GitHub Pages
5. Your app is now at: `https://yourusername.github.io/deploynotes/DeployNotes.html`

**Netlify (Free)**
1. Go to netlify.com and sign up
2. Drag and drop your DeployNotes.html file
3. Get instant URL like: `https://random-name.netlify.app`

**Other Options:**
- Vercel (free)
- Cloudflare Pages (free)
- Any web hosting you already have

---

## Installing on Your Phone

### iPhone (Safari)

1. **Open the link** in Safari (must be Safari, not Chrome)
2. **Tap the Share button** (square with arrow pointing up)
3. **Scroll down** and tap **"Add to Home Screen"**
4. **Name it** "DeployNotes" (or keep default)
5. **Tap "Add"**

The app icon now appears on your home screen and works offline!

---

### Android (Chrome)

1. **Open the link** in Chrome
2. **Tap the three-dot menu** (top right)
3. **Tap "Add to Home Screen"** or **"Install App"**
4. **Confirm** by tapping "Add"

The app icon now appears on your home screen and works offline!

---

### Desktop (Computer)

Simply bookmark the page or save the HTML file locally. Open in any browser.

---

## First-Time Setup

### Step 1: Create Your PIN

When you first open DeployNotes, you'll see the lock screen:

```
🔐
Create PIN
Create a 4-digit PIN to secure your data

[    ]

[1] [2] [3]
[4] [5] [6]
[7] [8] [9]
[Clear] [0] [⌫]
```

1. Enter a 4-digit PIN you'll remember
2. Enter it again to confirm
3. You're in!

**⚠️ Remember your PIN!** You'll need it every time you open the app.

---

### Step 2: Configure Settings

Tap **"⚙️ Settings & Cloud Setup"** at the bottom to configure:

| Setting | What to Enter |
|---------|---------------|
| **Default Deployment Location** | Your mission name (e.g., "Jamaica - Hurricane Melissa Relief") |
| **Cloud Storage URL** | Your HIPAA-compliant upload endpoint (optional) |
| **API Key / Token** | Authentication for cloud upload (optional) |
| **Organization ID** | Your org name (e.g., "BeVera Corps") |

Tap **"Save Settings"** when done.

---

## Using the App

### Daily Workflow

**Morning:**
1. Open DeployNotes → Enter PIN
2. Verify date and deployment day
3. Fill in your location (Parish, Community, Facility)

**Throughout the Day:**
- Add notes to each section as you work
- Capture photos/videos
- Add stories when you hear them
- Check health risks you observe

**End of Day:**
1. Complete the Reflection section
2. Review your entries
3. Upload to cloud (if wifi available) OR export locally
4. Tap "🔒 Lock App" when done

---

### Section-by-Section Guide

#### 📍 Deployment & Location
| Field | Example |
|-------|---------|
| Date | Auto-fills today |
| Deployment Day | Day 1, Day 2, etc. |
| Your Name | Reggie Johnson |
| Deployment Location | Jamaica - Hurricane Melissa Relief |
| Parish/Province/State | St. James |
| Community/Town | Catherine Hall |
| Facility/Site | Cornwall Regional Hospital |

---

#### 🚗 The Journey
Document what you experienced traveling and arriving:
- What did you see on the way?
- Road conditions?
- First impressions?

**Day Theme dropdown** - Choose the social media theme for that day to guide your storytelling.

---

#### 👥 The People
Write about who you met and interacted with.

**Consent Tracking:**
- ✅ **Total Consent** - Names, photos, quotes OK for public use
- ⚠️ **Partial Consent** - Anonymous only, no identifying info
- 🔒 **Internal Only** - Team communication only

Use the notes field to specify who gave what consent.

---

#### 💪 The Impact
**Quick Stats:**
- People reached
- Households visited
- Stories captured

**Activities Checkboxes:**
- WASH Education
- Leptospirosis/Disease Education
- Needs Assessment
- Supply Distribution
- Construction/Rebuild
- Debris Clearing
- Shelter Setup
- Training
- Medical
- Food Distribution

**Notes:** Describe what you accomplished.

---

#### 📸 Photos & Videos

**Capture Options:**
| Button | What It Does |
|--------|--------------|
| 📷 Take Photo | Opens camera |
| 🎥 Record Video | Opens video recorder |
| 🖼️ Choose from Gallery | Select existing files |

- Thumbnails appear in a grid
- Tap ✕ to remove
- Counter shows total files and size
- Add notes describing key media

**⚠️ Remember:** Get consent before photographing people!

---

#### 📖 Stories Captured

Tap **"+ Add Story"** for each human story you capture:

| Field | What to Enter |
|-------|---------------|
| Name/Pseudonym | Maria, or "Community Elder" |
| Role | Farmer, Mother, Health Worker, etc. |
| Consent | ✅ Full / ⚠️ Partial / 🔒 Internal |
| Their Story | What happened to them, how they're coping |
| Key Quote | Memorable words in their voice |
| Outcome | How help made a difference |

---

#### 🏥 Health Risks

Check all that apply:
- Standing Water
- Sanitation Issues
- Mosquito Breeding
- Unmet Medical Needs
- Contaminated Water
- Wounds/Injuries
- Respiratory Issues
- Mold Exposure
- Mental Health Concerns
- Vulnerable Elderly
- At-Risk Children
- Chronic Illness (no meds)

Add notes with specifics.

---

#### 💭 End of Day Reflection

**Real Talk Angle** - Choose a preset or enter your own:
- 😓 Harder than imagined
- 📋 Bureaucracy challenges
- 💔 Emotional toll
- 🌅 Finding hope
- 🙏 Gratitude
- 📚 Lesson learned
- ✏️ Enter my own angle...

Write your reflection: How did today affect you? What will you remember?

---

## Syncing & Exporting Data

### Status Indicators (Top of App)

| Indicator | Meaning |
|-----------|---------|
| 🟢 Online | Internet connected |
| 🟠 Offline | No internet - data saved locally |
| 🟢 Synced | All data uploaded |
| 🟠 Pending (3) | 3 items waiting to upload |

---

### Export Options

| Button | What It Does |
|--------|--------------|
| **☁️ Upload to Cloud** | Sends report + media to your configured endpoint |
| **📄 Export Local** | Downloads JSON file to your device |
| **🔒 Lock App** | Returns to PIN screen |
| **🗑️ Clear & Start Fresh** | Erases all data for new day |

---

### Recommended End-of-Day Process

**If you have wifi:**
1. Tap "☁️ Upload to Cloud"
2. Wait for "✅ Upload successful!"
3. Tap "🗑️ Clear & Start Fresh" for tomorrow

**If no wifi:**
1. Tap "📄 Export Local"
2. Save the JSON file
3. Optionally export media files
4. Upload to cloud storage manually when wifi available
5. Clear for tomorrow

---

## Settings & Configuration

Access settings by tapping **"⚙️ Settings & Cloud Setup"**

### Default Deployment Location
Set your mission name here - it auto-fills for every new report.

### Cloud Storage (HIPAA-Compliant)
To enable automatic uploads, you need a compliant storage endpoint:

| Service | Approximate Cost | Notes |
|---------|------------------|-------|
| Sync.com Business | ~$8/user/month | Easy setup, HIPAA compliant |
| Box Business | ~$15/user/month | HIPAA with BAA |
| AWS S3 | ~$5-20/month | Requires BAA signing |
| Azure Blob | ~$5-20/month | Requires BAA signing |
| Google Cloud | ~$5-20/month | Requires BAA signing |

Enter your endpoint URL and API key in settings.

### Reset PIN
If you need to change your PIN, use the "Reset PIN" button in settings.

---

## Troubleshooting

### "I forgot my PIN"
Unfortunately, for security reasons, there's no PIN recovery. You'll need to:
1. Clear the app's data (browser settings → clear site data)
2. Set up a new PIN
3. Previous data will be lost

**Tip:** Write your PIN somewhere secure!

---

### "My data disappeared"
Data is stored in your browser's local storage. This can be cleared if you:
- Clear browser data/cache
- Use private/incognito mode
- Uninstall and reinstall the app

**Solution:** Export data regularly!

---

### "Photos won't capture"
1. Make sure you've granted camera permissions
2. Try the "Choose from Gallery" option instead
3. Check available storage space

---

### "Upload failed"
1. Check your internet connection
2. Verify cloud URL and API key in settings
3. Data is saved locally - try again later
4. Use "Export Local" as backup

---

### "App won't install on home screen"
- **iPhone:** Must use Safari (not Chrome)
- **Android:** Must use Chrome (not other browsers)
- Try refreshing the page first

---

## Quick Reference Card

### Daily Checklist
```
□ Open app & enter PIN
□ Verify date and location
□ Document journey observations
□ Log people met + consent status
□ Record impact numbers & activities
□ Capture photos/videos (with consent!)
□ Add stories as you hear them
□ Note health risks observed
□ Write end-of-day reflection
□ Upload or export data
□ Lock app when done
```

### Keyboard Shortcuts (Desktop)
- `Ctrl/Cmd + S` - Triggers auto-save

### Support
For issues with the app, contact your deployment coordinator or IT support.

---

## Version Information

**App:** DeployNotes v1.0
**Created:** March 2026
**For:** BeVera Corps & Humanitarian Field Teams

---

*This guide can be printed for offline reference during deployments.*
