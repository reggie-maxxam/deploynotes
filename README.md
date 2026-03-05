# 📋 DeployNotes

**Secure, offline-capable field reporting for humanitarian deployments**

![Version](https://img.shields.io/badge/version-1.0.0-blue)
![License](https://img.shields.io/badge/license-MIT-green)
![PWA](https://img.shields.io/badge/PWA-ready-brightgreen)

---

## What is DeployNotes?

DeployNotes is a Progressive Web App (PWA) designed for humanitarian field workers, disaster relief teams, and anyone who needs to capture detailed daily reports in environments with limited connectivity.

### ✨ Key Features

- 🔐 **PIN Protected** - Secure access to sensitive field data
- 📴 **100% Offline** - Works without internet, syncs when available
- 📸 **Photo & Video Capture** - Document with media directly in the app
- ☁️ **HIPAA-Ready Upload** - Configure your own compliant cloud storage
- 📍 **Flexible Location Tracking** - Deployment, Parish, Community, Facility
- 📖 **Story Capture** - Document human stories with consent tracking
- 🏥 **Health Risk Logging** - Quick checkboxes for field observations
- 📱 **Mobile-First Design** - Optimized for phones in the field

---

## 🚀 Quick Start

### Use Hosted Version
Visit: **[https://yourusername.github.io/deploynotes](https://yourusername.github.io/deploynotes)**

### Install on Your Phone

**iPhone (Safari):**
1. Open the link in Safari
2. Tap the Share button (□↑)
3. Tap "Add to Home Screen"

**Android (Chrome):**
1. Open the link in Chrome
2. Tap the menu (⋮)
3. Tap "Add to Home Screen"

---

## 📁 Files Included

| File | Description |
|------|-------------|
| `index.html` | The DeployNotes app (main file) |
| `DeployNotes_User_Guide.md` | Complete user documentation |
| `DeployNotes_QuickStart.md` | Printable one-page reference |
| `DeployNotes_Technical_Overview.md` | Security & data handling details |

---

## 🔒 Security Overview

| Feature | Implementation |
|---------|----------------|
| Access Control | 4-digit PIN (hashed, never stored plain) |
| Data Storage | localStorage + IndexedDB (sandboxed) |
| Cloud Transfer | HTTPS with Bearer token auth |
| Offline Mode | Full functionality without internet |

**Note:** For HIPAA compliance, pair with a compliant cloud endpoint (AWS S3, Azure Blob, or Sync.com with BAA).

[See full security documentation →](DeployNotes_Technical_Overview.md)

---

## 📱 Screenshots

*App screenshots coming soon*

---

## 🛠️ Self-Hosting

### GitHub Pages (Free)
1. Fork this repository
2. Go to Settings → Pages
3. Select "Deploy from branch" → main
4. Your app is live at `https://yourusername.github.io/deploynotes`

### Other Hosting
Simply upload `index.html` to any web server. No build process required.

---

## 📖 Documentation

- [User Guide](DeployNotes_User_Guide.md) - Complete instructions
- [Quick Start](DeployNotes_QuickStart.md) - One-page reference
- [Technical Overview](DeployNotes_Technical_Overview.md) - Security & architecture

---

## 🤝 Contributing

Contributions welcome! Please read our contributing guidelines before submitting PRs.

---

## 📄 License

MIT License - see [LICENSE](LICENSE) file for details.

---

## 🙏 Acknowledgments

Built for humanitarian field teams worldwide.

Developed by **MAXXAM AI** - AI-Driven Automation Solutions

---

## 📞 Support

For questions or support, contact your deployment coordinator or open an issue in this repository.
