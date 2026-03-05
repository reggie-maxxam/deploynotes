# DeployNotes - Technical Overview
## Security, Data Storage & Export Process

---

## Table of Contents
1. [Security Architecture](#1-security-architecture)
2. [Data Storage Explained](#2-data-storage-explained)
3. [The Export Process](#3-the-export-process)
4. [Data Lifecycle](#4-data-lifecycle)
5. [Security Limitations (Be Honest With Clients)](#5-security-limitations)
6. [Client-Ready Talking Points](#6-client-ready-talking-points)

---

## 1. Security Architecture

### PIN Protection

**How It Works:**
```
User enters PIN → PIN is hashed → Hash is compared to stored hash → Access granted/denied
```

**Technical Details:**

| Component | Implementation |
|-----------|----------------|
| PIN Length | 4 digits (10,000 combinations) |
| Hashing | JavaScript hash function with salt |
| Salt | `'deploynotes-secure-salt-2024'` |
| Storage | Hash stored in `localStorage` as `deployNotesPinHash` |
| Attempts | Unlimited (no lockout in current version) |

**The Hash Function:**
```javascript
function hashPin(pin) {
    let hash = 0;
    const str = pin + 'deploynotes-secure-salt-2024';
    for (let i = 0; i < str.length; i++) {
        const char = str.charCodeAt(i);
        hash = ((hash << 5) - hash) + char;
        hash = hash & hash;
    }
    return hash.toString(16);
}
```

**What This Means:**
- The actual PIN is **never stored** - only its hash
- Even if someone accesses localStorage, they see: `"deployNotesPinHash": "3a7f2b1c"` (not the PIN)
- The salt prevents simple rainbow table attacks
- However, this is **not cryptographically strong** (see limitations)

---

## 2. Data Storage Explained

DeployNotes uses **two browser storage mechanisms**:

### A. localStorage (Text Data)

**What's Stored:**
| Key | Content |
|-----|---------|
| `deployNotesPinHash` | Hashed PIN |
| `deployNotesDraft` | All form data (JSON) |
| `deployNotesSettings` | Cloud URL, API key, org ID |
| `deployNotesPending` | Failed upload queue |

**Example of `deployNotesDraft`:**
```json
{
  "date": "2026-03-04",
  "day": "3",
  "name": "Reggie Johnson",
  "location": {
    "deployment": "Jamaica - Hurricane Melissa Relief",
    "parish": "St. James",
    "community": "Catherine Hall",
    "facility": "Community Center"
  },
  "journey": "Today we traveled through flooded roads...",
  "people": {
    "notes": "Met Maria, a local health worker...",
    "consent": {
      "full": true,
      "partial": false,
      "internal": false,
      "notes": "Maria gave full consent for photos"
    }
  },
  "impact": {
    "peopleReached": "45",
    "households": "12",
    "activities": ["WASH", "Construction", "Distro"]
  },
  "stories": [...],
  "reflection": "Today was challenging..."
}
```

**localStorage Characteristics:**
- **Capacity:** ~5-10 MB per domain
- **Persistence:** Survives browser close, device restart
- **Cleared by:** Manual clear, clearing browser data, uninstalling PWA
- **Encryption:** None (plain text)
- **Access:** Only this domain can read it

---

### B. IndexedDB (Media Files)

**What's Stored:**
- Photos (as base64-encoded strings)
- Videos (as base64-encoded strings)
- Metadata (filename, size, timestamp, upload status)

**Database Structure:**
```
Database: DeployNotesDB
├── Object Store: media
│   ├── id: 1709547823456.789
│   ├── type: "photo"
│   ├── name: "IMG_001.jpg"
│   ├── size: 2456789
│   ├── data: "data:image/jpeg;base64,/9j/4AAQ..."
│   ├── timestamp: "2026-03-04T14:30:00.000Z"
│   └── uploaded: false
│
└── Object Store: reports (reserved for future use)
```

**IndexedDB Characteristics:**
- **Capacity:** ~50-100+ MB (browser dependent, can request more)
- **Persistence:** Same as localStorage
- **Async:** Non-blocking read/write operations
- **Encryption:** None (plain data)

---

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

### Local Export (JSON)

**When user taps "📄 Export Local":**

```
1. collectData() gathers all form fields into object
2. JSON.stringify() converts to text
3. Blob created with MIME type 'application/json'
4. URL.createObjectURL() creates temporary download link
5. Programmatic click triggers download
6. File saves to device Downloads folder
7. URL revoked (cleanup)
```

**Output File Example:**
```
Day3_StJames_20260304_Reggie.json
```

**Media Export:**
- After JSON export, user is prompted: "Export 5 media files separately?"
- Each photo/video downloads as individual file
- Files named: `Day3_StJames_20260304_Reggie_media_1.jpg`

---

### Cloud Upload

**When user taps "☁️ Upload to Cloud":**

```
1. Check if cloudUrl configured (if not, show settings)
2. collectData() gathers all form fields
3. Attach media files (still base64 encoded)
4. Create payload object:
   {
     report: {...},
     media: [{...}, {...}],
     orgId: "BeVera Corps",
     uploadedAt: "2026-03-04T18:00:00Z"
   }
5. POST to configured endpoint with Authorization header
6. If success: mark media as uploaded, show success
7. If fail: save to pending queue, show error
```

**Request Structure:**
```javascript
fetch(settings.cloudUrl, {
    method: 'POST',
    headers: {
        'Content-Type': 'application/json',
        'Authorization': `Bearer ${settings.cloudApiKey}`
    },
    body: JSON.stringify(payload)
});
```

---

## 4. Data Lifecycle

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
│  Captures photo  →   Saves to IndexedDB    Medium              │
│                      (base64)               (unencrypted)       │
│       │                    │                                    │
│       ▼                    ▼                                    │
│  End of day              Data ready                             │
│       │                                                         │
│       ├──► Export Local → JSON file on     Medium              │
│       │                   device            (unencrypted file)  │
│       │                                                         │
│       ├──► Upload Cloud → Sent via HTTPS   Low-Medium          │
│       │                   to endpoint       (encrypted transit) │
│       │                                                         │
│       └──► Clear form  → localStorage &    None                │
│                          IndexedDB wiped    (data gone)         │
│                                                                 │
└─────────────────────────────────────────────────────────────────┘
```

### Auto-Save Behavior

```javascript
// Triggers auto-save:
- Every input change (debounced 500ms)
- Every select change (immediate)
- Every checkbox change (immediate)

// What gets saved:
- All text fields
- All checkboxes
- All dropdown selections
- Story entries
- NOT media (already in IndexedDB)
```

### Data Persistence Scenarios

| Scenario | Text Data | Media | PIN |
|----------|-----------|-------|-----|
| Close browser | ✅ Kept | ✅ Kept | ✅ Kept |
| Restart device | ✅ Kept | ✅ Kept | ✅ Kept |
| Clear browser cache | ❌ Lost | ❌ Lost | ❌ Lost |
| Clear site data | ❌ Lost | ❌ Lost | ❌ Lost |
| Uninstall PWA | ❌ Lost | ❌ Lost | ❌ Lost |
| Use incognito mode | ❌ Not saved | ❌ Not saved | ❌ Not saved |
| Different browser | ❌ Not shared | ❌ Not shared | ❌ Not shared |

---

## 5. Security Limitations (Be Honest With Clients)

### What DeployNotes IS:

✅ **PIN-gated access** - Prevents casual unauthorized viewing
✅ **Offline-capable** - Works without internet
✅ **Local-first** - Data stays on device until explicitly exported
✅ **Domain-sandboxed** - Other websites can't access the data
✅ **HTTPS upload** - Data encrypted during cloud transfer

### What DeployNotes IS NOT:

❌ **End-to-end encrypted** - Data is stored as plain text on device
❌ **Military-grade security** - PIN hash is basic, not bcrypt/argon2
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
| Phone stolen (unlocked) | ⚠️ Partial | PIN helps, but tech-savvy attacker could access storage |
| Malware on device | ❌ No | Malware with storage access can read data |
| Browser extension snooping | ❌ No | Extensions can access localStorage |
| Legal subpoena | ❌ No | Data is recoverable from device |
| Cloud endpoint compromised | ❌ No | Data at rest depends on cloud provider |

### For True HIPAA Compliance, You'd Need:

1. **At-rest encryption** - Encrypt localStorage/IndexedDB with user password
2. **Stronger hashing** - bcrypt or Argon2 for PIN
3. **PIN lockout** - Lock after 5 failed attempts
4. **Auto-lock timeout** - Lock after 5 min inactivity
5. **Secure enclave storage** - Use device's secure element (not possible in web)
6. **Audit logging** - Track all data access
7. **BAA with hosting provider** - Business Associate Agreement
8. **Third-party security audit**

---

## 6. Client-Ready Talking Points

### For Sales/Demo Conversations:

**"How secure is the data?"**
> "DeployNotes uses PIN protection to prevent unauthorized access. Data is stored locally on the device - it never touches our servers unless you explicitly upload it to your own cloud storage. The PIN is cryptographically hashed, so even if someone accessed the browser storage, they wouldn't see the actual PIN. For sensitive deployments, we recommend using this alongside device-level encryption (which most modern phones have by default)."

**"Is it HIPAA compliant?"**
> "DeployNotes is designed to be HIPAA-ready, but compliance depends on the full workflow. The app itself stores data locally with PIN protection. For HIPAA compliance, you'd pair it with a HIPAA-compliant cloud endpoint (like AWS S3 with a BAA or Sync.com Business) and ensure your team follows proper device security protocols. We can help you set up an end-to-end compliant workflow."

**"What happens if they lose their phone?"**
> "The data is protected by the app's PIN and the device's lock screen - that's two layers of protection. If someone finds the phone, they'd need to bypass both. Additionally, we recommend enabling remote wipe on all field devices, and having team members export/upload data daily so the device only holds one day's data at most."

**"How does offline mode work?"**
> "Everything is stored directly on the phone using browser storage technology. Team members can capture notes, photos, and videos all day without any internet connection. Data auto-saves continuously. When wifi becomes available, they tap one button to upload everything to your cloud storage. If the upload fails, the data stays safe locally until they try again."

**"What if someone clears their browser data?"**
> "That would erase the stored data - which is why we emphasize the daily export/upload workflow. At the end of each day, the data should be uploaded to cloud storage or exported as a file. The local copy is a working draft; the export is the permanent record."

---

## Quick Reference: Where's My Data?

```
┌────────────────────────────────────────────────────────────┐
│                    DEPLOYNOTES DATA MAP                    │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  ON THE DEVICE (Browser Storage)                           │
│  ────────────────────────────────                          │
│  localStorage:                                             │
│    • deployNotesPinHash    → Your hashed PIN               │
│    • deployNotesDraft      → All form text data            │
│    • deployNotesSettings   → Cloud URL, API key            │
│    • deployNotesPending    → Failed upload queue           │
│                                                            │
│  IndexedDB (DeployNotesDB):                                │
│    • media store           → Photos & videos (base64)      │
│                                                            │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  EXPORTED FILES (On Device)                                │
│  ─────────────────────────                                 │
│  Downloads folder:                                         │
│    • Day3_Location_Date_Name.json   → Full report          │
│    • Day3_..._media_1.jpg           → Individual photos    │
│    • Day3_..._media_2.mp4           → Individual videos    │
│                                                            │
├────────────────────────────────────────────────────────────┤
│                                                            │
│  CLOUD STORAGE (Your Endpoint)                             │
│  ─────────────────────────────                             │
│  Whatever you configure:                                   │
│    • POST request with JSON body                           │
│    • Includes report data + base64 media                   │
│    • Authorization via Bearer token                        │
│                                                            │
└────────────────────────────────────────────────────────────┘
```

---

## Recommendations for Client Deployments

### Before Deployment:
1. Enable device encryption on all field phones
2. Set up remote wipe capability
3. Configure a HIPAA-compliant cloud endpoint
4. Train team on daily export/upload workflow
5. Establish PIN policy (don't use 1234, 0000, etc.)

### During Deployment:
1. Export/upload data at end of each day
2. Clear form after successful upload
3. Keep devices physically secure
4. Don't share PINs

### After Deployment:
1. Verify all data uploaded to cloud
2. Clear all devices before returning
3. Revoke cloud API keys if needed
4. Archive data according to retention policy

---

*Document Version: 1.0 | March 2026 | For Internal Use & Client Education*
