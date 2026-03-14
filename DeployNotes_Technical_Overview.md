# DeployNotes - Technical Overview
## Security, Data Storage & Export Process

**Version:** 3.1 | March 2026

---

## Table of Contents
1. [Security Architecture](#1-security-architecture)
2. [Data Storage Explained](#2-data-storage-explained)
3. [The Export Process](#3-the-export-process)
4. [Cloud Upload with n8n](#4-cloud-upload-with-n8n)
5. [Data Lifecycle](#5-data-lifecycle)
6. [Security Limitations (Be Honest With Clients)](#6-security-limitations)
7. [Client-Ready Talking Points](#7-client-ready-talking-points)

---

## 1. Security Architecture

### PIN Protection

**How It Works:**
```
User enters PIN → PIN stored in localStorage → Compared on next unlock → Access granted/denied
```

**Technical Details:**

| Component | Implementation |
|-----------|----------------|
| PIN Length | 4 digits (10,000 combinations) |
| Storage | Plain text in `localStorage` as `dn_pin` |
| Attempts | Unlimited (no lockout in current version) |

**What This Means:**
- Simple PIN protection prevents casual access
- Not cryptographically secured (see limitations section)
- Suitable for field use where convenience matters

---

## 2. Data Storage Explained

DeployNotes uses **localStorage** for all data (text + base64-encoded media).

### localStorage Keys

| Key | Content |
|-----|---------|
| `dn_pin` | 4-digit PIN (plain text) |
| `dn_draft` | All form data including photos, stories, signature (JSON) |
| `dn_settings` | Cloud URL, API key, org ID, deployment name |

### Draft Data Structure (`dn_draft`)

```json
{
  "date": "2026-03-14",
  "day": "3",
  "name": "Reggie Johnson",
  "deploy": "Jamaica - Hurricane Relief",
  "parish": "St. James",
  "comm": "Catherine Hall",
  "facility": "Community Center",
  "theme": "The Journey Begins",
  "journey": "Today we traveled through flooded roads...",
  "people": "Met Maria, a local health worker...",
  "consentFull": true,
  "consentPartial": false,
  "consentInternal": false,
  "consentNotes": "Maria gave full consent for photos",
  "peopleNum": 45,
  "households": 12,
  "storiesNum": 3,
  "activities": ["Assess", "Distribute", "WASH"],
  "impact": "Distributed supplies to 45 people...",
  "healthRisks": ["Water", "Sanitation"],
  "health": "Water quality concerns noted...",
  "angle": "Harder than imagined",
  "reflection": "Today was challenging...",
  "sigName": "Reggie Johnson",
  "sigDate": "2026-03-14",
  "photos": [
    { "type": "photo", "data": "data:image/jpeg;base64,/9j/4AAQ..." }
  ],
  "consentPhotos": [
    { "type": "consent", "data": "data:image/jpeg;base64,/9j/4AAQ..." }
  ],
  "signature": "data:image/png;base64,iVBORw0...",
  "stories": [
    {
      "name": "Maria",
      "role": "Community Health Worker",
      "consent": "Full",
      "text": "Maria has been working in this community for 15 years...",
      "quote": "We just want clean water for our children."
    }
  ]
}
```

### localStorage Characteristics

| Property | Value |
|----------|-------|
| **Capacity** | ~5-10 MB per domain |
| **Persistence** | Survives browser close, device restart |
| **Cleared by** | Manual clear, clearing browser data, uninstalling PWA |
| **Encryption** | None (plain text) |
| **Access** | Only this domain can read it |

### Storage Location on Device

**Desktop Browsers:**
```
Chrome:  ~/Library/Application Support/Google/Chrome/Default/Local Storage/
Firefox: ~/Library/Application Support/Firefox/Profiles/xxx/storage/
Safari:  ~/Library/Safari/LocalStorage/
```

**Mobile (when installed as PWA):**
```
iOS:     Sandboxed within Safari/app container
Android: Sandboxed within Chrome/app container
```

**Key Point:** Data is sandboxed - other apps cannot access it, but it's not encrypted at rest.

---

## 3. The Export Process

### Local Export (HTML Report)

**When user taps "📄 Export Report":**

```
1. collectReportData() gathers all form fields
2. HTML template built with embedded base64 images
3. Blob created with MIME type 'text/html'
4. URL.createObjectURL() creates temporary download link
5. Programmatic click triggers download
6. File saves to device Downloads folder
```

**Output File:**
```
DeployNotes_Report_2026-03-14.html
```

The HTML report contains:
- All form data formatted with styling
- Photos embedded as base64 (viewable offline)
- Consent photos embedded
- Digital signature embedded
- Professional layout suitable for printing

### JSON Backup

**When user taps "💾 Backup JSON":**

```
1. collectReportData() gathers all data
2. JSON.stringify() converts to text
3. Downloads as .json file
```

**Output File:**
```
DeployNotes_Backup_2026-03-14.json
```

---

## 4. Cloud Upload with n8n

### Overview

DeployNotes integrates with **n8n** (workflow automation) to upload reports to **Google Drive**. This creates an organized folder structure with all report files.

### Architecture

```
┌─────────────────┐       HTTPS POST        ┌─────────────────┐
│   DeployNotes   │ ───────────────────────► │   n8n Webhook   │
│      PWA        │   JSON + Base64 Media    │    Workflow     │
└─────────────────┘                          └────────┬────────┘
                                                      │
                                                      ▼
                                             ┌─────────────────┐
                                             │  Google Drive   │
                                             │     Folder      │
                                             └─────────────────┘
```

### Upload Payload Structure

**What DeployNotes sends:**

```javascript
{
  report: {
    date: "2026-03-14",
    day: 1,
    name: "Reggie Johnson",           // Reporter name
    deploy: "Jamaica Hurricane Relief", // NOT "deployment"
    parish: "St. James",
    comm: "Montego Bay",              // NOT "community"
    facility: "Verney House Resort",
    theme: "The Journey Begins",
    journey: "...",
    people: "...",
    consentFull: true,
    consentPartial: false,
    consentInternal: false,
    consentNotes: "...",
    peopleNum: 25,
    households: 10,
    storiesNum: 2,
    activities: ["Assess", "Distribute"],
    impact: "...",
    mediaNotes: "",
    healthRisks: ["Water", "Sanitation"],
    health: "...",
    angle: "Harder than imagined",
    reflection: "...",
    sigName: "Reggie Johnson",
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
    { name: "Maria", role: "CHW", consent: "Full", text: "...", quote: "..." }
  ],
  orgId: "MAXXAM AI",
  uploadedAt: "2026-03-14T17:34:01.151Z"
}
```

**Critical Field Names:**
| App Field | Payload Path | Notes |
|-----------|--------------|-------|
| Deployment | `body.report.deploy` | NOT `deployment` |
| Community | `body.report.comm` | NOT `community` |
| Reporter | `body.report.name` | Reporter's name |
| Field Photos | `body.media[]` | Array of photo objects |
| Consent Photos | `body.consentMedia[]` | Array of consent photos |
| Signature | `body.signature` | Base64 PNG string |

### n8n Workflow Architecture

```
Receive DeployNotes Upload (Webhook)
           │
           ▼
   Extract & Prepare Data (Code)
           │
           ▼
   Create Drive Folder (Google Drive)
           │
           ▼
     Store Folder ID (Code)
           │
     ┌─────┴─────┐
     ▼           ▼
Create JSON   Create HTML
  Backup        Report
     │           │
     ▼           ▼
 Upload       Upload HTML
  JSON          Report
                 │
                 ▼
           Check Photos?
            │        │
           Yes       No
            │        │
            ▼        ▼
      Split Photos   Success Response
            │
            ▼
   Prepare Photo Binary (loop)
            │
            ▼
       Upload Photo
            │
            ▼
     Aggregate Results
            │
            ▼
      Success Response
```

### What Gets Created in Google Drive

**Folder:** `2026-03-14_Day1_Jamaica_Hurricane_Relief_Reggie_Johnson`

| File | Description |
|------|-------------|
| `{folder}_backup.json` | Full JSON backup of all data |
| `{folder}_report.html` | Formatted HTML report with embedded images |
| `photo_1.jpg` | Field photo #1 |
| `photo_2.jpg` | Field photo #2 |
| `consent_1.jpg` | Consent form photo |
| `signature.png` | Digital signature |

### Setup Requirements

1. **n8n instance** (self-hosted or cloud)
2. **Google Drive OAuth2** credentials in n8n
3. **Workflow import:** `n8n/DeployNotes_CloudUpload_v3.json`
4. **Connect credentials** to all Google Drive nodes
5. **Activate workflow** and copy Production webhook URL
6. **Configure DeployNotes** Settings with webhook URL

### Request Structure

```javascript
fetch(settings.cloudUrl, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${settings.cloudApiKey}`  // Optional
    },
    body: JSON.stringify(payload)
});
```

---

## 5. Data Lifecycle

### Timeline of Data

```
┌─────────────────────────────────────────────────────────────────┐
│                        DATA LIFECYCLE                           │
├─────────────────────────────────────────────────────────────────┤
│                                                                 │
│  USER ACTION          STORAGE STATE         RISK LEVEL          │
│  ───────────          ─────────────         ──────────          │
│                                                                 │
│  Opens app            PIN check             Low                 │
│       │                    │                                    │
│       ▼                    ▼                                    │
│  Enters data     →   Auto-saves to         Medium              │
│                      localStorage           (unencrypted)       │
│       │                    │                                    │
│       ▼                    ▼                                    │
│  Captures photo  →   Saves to localStorage  Medium              │
│                      (base64 in dn_draft)   (unencrypted)       │
│       │                    │                                    │
│       ▼                    ▼                                    │
│  End of day              Data ready                             │
│       │                                                         │
│       ├──► Export HTML → File on device    Medium              │
│       │                  (self-contained)   (unencrypted file)  │
│       │                                                         │
│       ├──► Upload Cloud → Sent via HTTPS   Low                 │
│       │                   to n8n webhook    (encrypted transit) │
│       │                   → Google Drive    (encrypted at rest) │
│       │                                                         │
│       └──► Clear form  → localStorage       None                │
│                          cleared            (data gone)         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Auto-Save Behavior

```javascript
// Triggers auto-save:
- Every input change (debounced)
- Every select change (immediate)
- Every checkbox change (immediate)
- Photo capture (immediate)
- Signature capture (immediate)

// What gets saved to dn_draft:
- All text fields
- All checkboxes
- All dropdown selections
- Photos (base64)
- Consent photos (base64)
- Signature (base64)
- Story entries
```

### Data Persistence Scenarios

| Scenario | Data | PIN |
|----------|------|-----|
| Close browser | ✅ Kept | ✅ Kept |
| Restart device | ✅ Kept | ✅ Kept |
| Clear browser cache | ❌ Lost | ❌ Lost |
| Clear site data | ❌ Lost | ❌ Lost |
| Uninstall PWA | ❌ Lost | ❌ Lost |
| Use incognito mode | ❌ Not saved | ❌ Not saved |
| Different browser | ❌ Not shared | ❌ Not shared |

---

## 6. Security Limitations (Be Honest With Clients)

### What DeployNotes IS:

✅ **PIN-gated access** - Prevents casual unauthorized viewing  
✅ **Offline-capable** - Works without internet  
✅ **Local-first** - Data stays on device until explicitly exported  
✅ **Domain-sandboxed** - Other websites can't access the data  
✅ **HTTPS upload** - Data encrypted during cloud transfer  
✅ **Google Drive integration** - Encrypted at rest in Google's infrastructure  

### What DeployNotes IS NOT:

❌ **End-to-end encrypted** - Data is stored as plain text on device  
❌ **Military-grade security** - PIN is plain text, not hashed  
❌ **HIPAA-certified** - The app itself has no certification  
❌ **Protected if device is compromised** - Root access = data access  
❌ **Protected against screenshot/screen recording**  
❌ **Compliant with PIN lockout requirements** - Unlimited attempts  

### Real-World Risk Assessment

| Threat | Protected? | Notes |
|--------|------------|-------|
| Curious coworker picks up phone | ✅ Yes | PIN blocks casual access |
| Someone guesses PIN | ⚠️ Partial | No lockout, but 10,000 combos |
| Phone stolen (locked) | ✅ Yes | Device lock + app PIN |
| Phone stolen (unlocked) | ⚠️ Partial | PIN helps, but storage accessible |
| Malware on device | ❌ No | Malware can read localStorage |
| Browser extension snooping | ❌ No | Extensions can access localStorage |
| Legal subpoena | ❌ No | Data is recoverable from device |
| Cloud endpoint compromised | ⚠️ Partial | Google Drive has encryption |

### For True HIPAA Compliance, You'd Need:

1. **At-rest encryption** - Encrypt localStorage with user password
2. **Stronger PIN security** - bcrypt hashing + lockout
3. **Auto-lock timeout** - Lock after 5 min inactivity
4. **BAA with Google** - Business Associate Agreement for Workspace
5. **Audit logging** - Track all data access
6. **Third-party security audit**

---

## 7. Client-Ready Talking Points

### For Sales/Demo Conversations:

**"How secure is the data?"**
> "DeployNotes uses PIN protection to prevent unauthorized access. Data is stored locally on the device - it never touches external servers unless you explicitly upload it to your own Google Drive. For sensitive deployments, we recommend using this alongside device-level encryption (which most modern phones have by default)."

**"Is it HIPAA compliant?"**
> "DeployNotes is designed to be HIPAA-ready, but compliance depends on the full workflow. For HIPAA compliance, you'd use Google Workspace with a signed BAA, ensure your team follows proper device security protocols, and set up appropriate access controls. We can help you configure a compliant workflow."

**"What happens if they lose their phone?"**
> "The data is protected by the app's PIN and the device's lock screen - that's two layers of protection. We recommend enabling remote wipe on all field devices, and having team members upload data daily so the device only holds minimal data."

**"How does offline mode work?"**
> "Everything is stored directly on the phone using browser storage. Team members can capture notes, photos, and signatures all day without any internet connection. Data auto-saves continuously. When wifi becomes available, they tap one button to upload everything to Google Drive."

**"What if someone clears their browser data?"**
> "That would erase the stored data - which is why we emphasize the daily upload workflow. At the end of each day, the data should be uploaded to Google Drive. The local copy is a working draft; the cloud copy is the permanent record."

---

## Quick Reference: Where's My Data?

```
┌────────────────────────────────────────────────────────────┐
│                    DEPLOYNOTES DATA MAP                    │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  ON THE DEVICE (Browser localStorage)                      │
│  ────────────────────────────────────                      │
│    • dn_pin              → 4-digit PIN                     │
│    • dn_draft            → All form data + photos (JSON)   │
│    • dn_settings         → Cloud URL, API key, org name    │
│                                                            │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  EXPORTED FILES (On Device Downloads)                      │
│  ────────────────────────────────────                      │
│    • DeployNotes_Report_YYYY-MM-DD.html  → HTML report     │
│    • DeployNotes_Backup_YYYY-MM-DD.json  → JSON backup     │
│                                                            │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  GOOGLE DRIVE (via n8n)                                    │
│  ──────────────────────                                    │
│  Folder: {date}_Day{N}_{deploy}_{name}                     │
│    • {folder}_backup.json    → Full JSON backup            │
│    • {folder}_report.html    → Formatted HTML report       │
│    • photo_1.jpg, photo_2.jpg...  → Field photos          │
│    • consent_1.jpg...        → Consent form photos         │
│    • signature.png           → Digital signature           │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

---

## Recommendations for Client Deployments

### Before Deployment:
1. Enable device encryption on all field phones
2. Set up remote wipe capability
3. Configure n8n workflow with Google Drive
4. Train team on daily upload workflow
5. Establish PIN policy (don't use 1234, 0000, etc.)

### During Deployment:
1. Upload data at end of each day
2. Clear form after successful upload
3. Keep devices physically secure
4. Don't share PINs

### After Deployment:
1. Verify all data uploaded to Google Drive
2. Clear all devices before returning
3. Archive data according to retention policy

---

*Document Version: 3.1 | March 2026 | MAXXAM AI*
