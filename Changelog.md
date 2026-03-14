# Changelog

All notable changes to DeployNotes will be documented in this file.

## [3.1] - 2026-03-14

### Added
- ☁️ Cloud upload functionality via n8n webhook
- Cloud Storage URL and API Key fields in Settings
- HIPAA compliance notice in Settings
- Loading state during cloud upload

### Fixed
- Photo detection in n8n workflow (data path: `body.media[]`)
- Folder naming now uses correct field names (`deploy`, `comm`, `name`)
- Split Photos node output format for n8n compatibility

### Technical
- Upload payload uses Bearer token authentication
- HTML report embeds all images as base64
- n8n workflow creates organized folder structure in Google Drive

---

## [3.0] - 2026-03-08

### Added
- ✍️ Digital signature capture with canvas pad
- 📖 Stories section with add/remove functionality
- Story fields: name, role, consent level, text, quote
- 🏥 Health Risks section with checkboxes
- Health risk notes field
- Consent documentation photo capture
- Reflection angle dropdown

### Changed
- Redesigned export to embed all images inline
- Improved HTML report styling with sections
- Updated button color scheme (blue/grey)

### Fixed
- Signature pad canvas sizing
- Export blank page issue (switched to array.push/join pattern)
- Dropdown text readability on dark backgrounds

---

## [2.0] - 2026-03-07

### Added
- 📸 Photo and video capture via camera
- Gallery upload support
- Media grid display with delete
- 📋 Consent form section
- Consent checkboxes (Full/Partial/Internal)
- 🔐 PIN lock screen
- Auto-save to localStorage

### Changed
- Dark theme for field visibility
- Responsive layout for mobile

---

## [1.0] - 2026-03-06

### Added
- 🚀 Initial release
- Location section (date, day, deployment, parish, community, facility)
- Journey section with theme dropdown
- People section
- Impact section with stats
- Basic HTML export
- JSON backup functionality
- Settings modal
