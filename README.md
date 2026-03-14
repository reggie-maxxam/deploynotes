# DeployNotes PWA

**Version:** 3.1  
**Live URL:** [https://reggie-maxxam.github.io/deploynotes/](https://reggie-maxxam.github.io/deploynotes/)  
**Repository:** [https://github.com/reggie-maxxam/deploynotes](https://github.com/reggie-maxxam/deploynotes)

A Progressive Web App (PWA) for field deployment reporting, designed for humanitarian relief workers, disaster response teams, and field researchers. Capture stories, photos, consent documentation, and impact data — all offline-capable with cloud sync.

---

## Features

| Feature | Description |
|---------|-------------|
| 📱 **PWA / Offline-First** | Install on any device, works without internet |
| 🔐 **PIN Lock** | 4-digit PIN protection for sensitive field data |
| 📸 **Photo Capture** | Camera integration + gallery upload for field photos |
| ✍️ **Digital Signatures** | Canvas-based signature capture for consent forms |
| 📖 **Story Collection** | Structured story capture with consent levels |
| ☁️ **Cloud Upload** | Sync to Google Drive via n8n webhook |
| 📄 **HTML Export** | Self-contained report with embedded images |
| 💾 **JSON Backup** | Full data backup for recovery |
| 🔄 **Auto-Save** | Continuous draft saving to localStorage |

---

## Report Sections

1. 📍 **Location** — Date, day number, deployment name, parish, community, facility
2. 🚗 **Journey** — Theme selection, journey narrative
3. 👥 **People** — People met, consent checkboxes (Full/Partial/Internal)
4. 💪 **Impact** — Stats (people served, households, stories), activities, impact description
5. 📸 **Photos & Videos** — Camera capture, media grid, notes
6. ✍️ **Consent Form** — Photo upload, consent banner, signature pad, name/date
7. 📖 **Stories** — Add/remove story entries with name, role, text, quote
8. 🏥 **Health Risks** — Risk checkboxes + notes
9. 💭 **Reflection** — Angle selection, reflection narrative

---

## Technical Architecture

### Stack
- **Frontend:** Vanilla HTML5, CSS3, JavaScript (ES6+)
- **Storage:** localStorage (auto-save drafts, settings, PIN)
- **Media:** Base64 encoding for photos/signatures
- **PWA:** Service Worker ready (add manifest.json for full PWA)
- **Cloud Sync:** n8n webhook integration

### File Structure
```
deploynotes/
├── index.html          # Main application (single-file PWA)
├── README.md           # This file
├── CHANGELOG.md        # Version history
└── n8n/
    └── DeployNotes_CloudUpload_v3.json  # n8n workflow for cloud sync
```

### Data Storage (localStorage)

| Key | Purpose |
|-----|---------|
| `dn_pin` | 4-digit PIN (plain text) |
| `dn_draft` | Auto-saved form data (JSON with photos, stories, signature) |
| `dn_settings` | Settings (deployment name, org, cloud URL, API key) |

### Cloud Upload Payload Structure
```javascript
{
  report: {
    date: "2026-03-14",
    day: "1",
    name: "Reporter Name",
    deploy: "Deployment Name",
    parish: "Parish",
    comm: "Community",
    facility: "Facility",
    theme: "The Journey Begins",
    journey: "Journey narrative...",
    people: "People description...",
    consentFull: true,
    consentPartial: false,
    consentInternal: false,
    consentNotes: "Notes...",
    peopleNum: 25,
    households: 10,
    storiesNum: 3,
    activities: ["Assess", "Distribute"],
    impact: "Impact description...",
    healthRisks: ["Water", "Sanitation"],
    health: "Health notes...",
    angle: "Harder than imagined",
    reflection: "Reflection text...",
    sigName: "Signature Name",
    sigDate: "2026-03-14"
  },
  media: [
    { type: "photo", data: "data:image/jpeg;base64,..." }
  ],
  consentMedia: [
    { type: "consent", data: "data:image/jpeg;base64,..." }
  ],
  signature: "data:image/png;base64,...",
  stories: [
    { name: "Person", role: "Role", consent: "Full", text: "...", quote: "..." }
  ],
  orgId: "Organization Name",
  uploadedAt: "2026-03-14T17:30:00.000Z"
}
```

---

## Installation

### Option 1: Use Live Version
Visit [https://reggie-maxxam.github.io/deploynotes/](https://reggie-maxxam.github.io/deploynotes/)

### Option 2: Self-Host
```bash
git clone https://github.com/reggie-maxxam/deploynotes.git
```
Serve `index.html` via any web server (GitHub Pages, Netlify, nginx, etc.)

### Option 3: Local Development
```bash
git clone https://github.com/reggie-maxxam/deploynotes.git
cd deploynotes
npx serve .
# or: python -m http.server 8000
```

---

## Cloud Upload Setup (n8n + Google Drive)

DeployNotes can sync reports to Google Drive via an n8n webhook workflow.

### Prerequisites
- n8n instance (self-hosted or cloud)
- Google Drive OAuth2 credentials configured in n8n

### Setup Steps

1. **Import the n8n workflow**
   - File: `n8n/DeployNotes_CloudUpload_v3.json`
   - In n8n: Settings → Import Workflow → Upload JSON

2. **Connect Google Drive credentials** to these nodes:
   - Create Drive Folder
   - Upload JSON
   - Upload HTML Report
   - Upload Photo

3. **Activate the workflow** and copy the **Production** webhook URL

4. **Configure DeployNotes**
   - Tap ⚙️ **Settings**
   - Paste webhook URL into **Cloud Storage URL**
   - Save

5. **Test** — Fill out a report and tap **☁️ Upload to Cloud**

### What Gets Uploaded

| File | Description |
|------|-------------|
| `{folder}_backup.json` | Full JSON backup of all data |
| `{folder}_report.html` | Formatted HTML report with embedded images |
| `photo_1.jpg`, etc. | Field photos |
| `consent_1.jpg`, etc. | Consent form photos |
| `signature.png` | Digital signature |

### Folder Naming Convention
```
{date}_Day{day}_{deployment}_{reporter}
```
Example: `2026-03-14_Day1_Jamaica_Relief_Reginald_Johnson`

### n8n Workflow Architecture
```
Webhook
    ↓
Extract & Prepare Data
    ↓
Create Drive Folder
    ↓
Store Folder ID
    ↓
    ├── Create JSON Backup → Upload JSON
    │
    └── Create HTML Report → Upload HTML Report
                                    ↓
                              Check Photos?
                               ↓        ↓
                              Yes       No
                               ↓         ↓
                          Split Photos   Success Response
                               ↓
                      Prepare Photo Binary
                               ↓
                          Upload Photo
                               ↓
                       Aggregate Results
                               ↓
                       Success Response
```

---

## Usage Guide

### First Time Setup
1. Open the app
2. Set a 4-digit PIN
3. Configure settings (deployment name, organization, cloud URL)

### Creating a Report
1. Unlock with PIN
2. Fill sections: Location → Journey → People → Impact → Photos → Consent → Stories → Health → Reflection
3. Add photos and signatures as needed
4. Upload or export when ready

### Saving Options
| Action | Result |
|--------|--------|
| **Auto-save** | Data saves continuously to device |
| **Export Report** | Downloads HTML file |
| **Backup JSON** | Downloads JSON backup |
| **Upload to Cloud** | Syncs to Google Drive |

### Viewing HTML Reports
1. Download `.html` file from Google Drive
2. Open in any web browser
3. Print to PDF if needed (Ctrl/Cmd + P → Save as PDF)

---

## Security & Privacy

### Data Storage
- All data stored locally in browser localStorage
- No data sent externally except configured cloud sync
- PIN protection for access control

### HIPAA Considerations
For HIPAA-compliant deployments:
- Use Google Workspace with signed BAA
- Host n8n on HIPAA-compliant infrastructure
- Enable SSL/TLS encryption
- Configure access logging

### Clearing Data
| Action | Effect |
|--------|--------|
| **Clear Media** | Removes photos only |
| **Clear All** | Removes all report data (keeps settings/PIN) |
| **Browser Clear** | Complete reset |

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| Photos not uploading | Check n8n workflow uses `body.media[]` path |
| Cloud upload fails | Verify webhook URL is Production (not Test), workflow is Active |
| PIN forgotten | Clear browser site data, set new PIN (clears all data) |
| Export shows blank | Ensure at least one section is filled |

---

## Changelog

### v3.1 (2026-03-14)
- ☁️ Cloud upload to Google Drive via n8n
- 🔧 Fixed photo detection in workflow
- 📝 Corrected data field mappings

### v3.0 (2026-03-08)
- ✍️ Digital signature capture
- 📖 Story collection with consent
- 🏥 Health risks section
- 📄 HTML export with embedded images

### v2.0 (2026-03-07)
- 📸 Photo/video capture
- 📋 Consent form section
- 🔐 PIN lock

### v1.0 (2026-03-06)
- 🚀 Initial release

---

## License

MIT License

---

## Credits

**Developed by:** MAXXAM AI  
**Website:** [maxxam.ai](https://maxxam.ai)

Built for humanitarian field workers who need reliable, offline-capable tools for capturing stories and impact data in challenging environments.
