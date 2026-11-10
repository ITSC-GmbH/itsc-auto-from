---
subject : "Browser extensions"
title : "ITSC Auto-From (Thunderbird MailExtension)"
author : "ITSC IT Systems Consulting GmbH"
abstract: "Automatically assigns the original recipient address (TO) as the sender address (FROM) when replying to emails if no matching identity exists for the email account."
keywords: ["Thunderbird"]
lang: "en"
---

Language / Sprache:

- 🇬🇧 English

- 🇩🇪 [Deutsch](./README.de.md)

**ITSC Auto-From** is a Thunderbird extension (WebExtension / MailExtension based on Manifest V3) that automatically sets the original recipient address (`TO`) as the sender address (`FROM`) when replying to an email if no exact matching identity exists for the email account.

# Problem & Purpose

To prevent spam, individual email addresses are often configured for different communication partners or services (e.g., `partner-service@mydomain.com`). All these emails are collected into a single shared IMAP account.

- If a dedicated identity exists in Thunderbird for an address, Thunderbird automatically selects it when replying.
- If **no** matching identity exists, Thunderbird would previously fall back to the account's default sender address, requiring manual entry of the correct partner address.

**Solution:** When composing a reply, this add-on checks whether a matching identity exists. If not, it automatically takes over the original message's `TO` address as the sender address (`FROM`).

# Features

1. **Automatic Sender Assignment (FROM)**:
   - Detects replies to incoming messages.
   - Compares recipient addresses (`TO`) against all configured identities of the account.
   - If Thunderbird finds no matching identity, the `TO` address is automatically assigned as the sender (`FROM`).
   - Optionally inherits the configured display name of the default identity (e.g., `John Doe <partner-alias@mydomain.com>`).

2. **Visual Signaling & Popup Controls**:
   - **Toolbar Badge**: Displays a green `AUTO` badge on the add-on icon in the compose window.
   - **Tooltip / Infobar**: Displays the assigned sender address.
   - **Status Popup**: Clicking the toolbar icon opens a detailed popup showing the original `TO` address, assigned `FROM` address, original Thunderbird sender, and account name.
   - **Undo Function ("Undo")**: When automatic sender assignment is active, clicking the "Undo" button restores Thunderbird's original sender address and changes the status to standard identity.
   - **Manual Override ("Override")**: When using the standard identity, clicking the "Override" button re-applies the automatic sender assignment according to add-on rules.
   - **System / Desktop Notification**: Can optionally be enabled in settings.

3. **Per-Account Control**:
   - Functionality can be enabled or disabled per Thunderbird email account in the add-on settings.
   - Enabled for all accounts by default.

4. **Internationalization (i18n)**:
   - Full support for **German** (`de`) and **English** (`en`).

# Configuration

The add-on settings allow enabling or disabling automatic sender assignment individually for each email account and customizing the visual signaling behavior.

## Opening Settings in Thunderbird

There are two ways to open the settings page:

1. **Via the Add-ons Manager (Standard Way)**:
   - In Thunderbird, open **Tools** (or the application menu `≡`) -> **Add-ons and Themes** (keyboard shortcut: `Ctrl + Shift + A` / `Cmd + Shift + A` on macOS).
   - Select **Extensions** in the left sidebar.
   - Locate the **ITSC Auto-From** entry.
   - Click either directly on the **wrench/gear icon** (Settings) or on the **three dots menu (`...`)** and select **Options** / **Preferences**.
   - The settings page opens in a new tab in Thunderbird.

2. **Via the Compose Window (Quick Access)**:
   - In the toolbar of the compose window, click the add-on icon (**ITSC Auto-From**).
   - In the opened status popup, click the **Open Settings** button at the bottom.

## Configuration Options

The settings page offers the following options:

### General Settings
- **Enable by default for new accounts**:
  - Specifies whether newly added email accounts should be enabled for automatic sender assignment by default.
- **Keep identity display name**:
  - When enabled (default), the real name of the default identity is preserved (e.g., `John Doe <partner@example.com>`). When disabled, only the plain email address is used.
- **Show badge in compose window**:
  - Shows a green `AUTO` badge on the toolbar icon in the compose window when a sender address has been automatically adjusted.
- **Show desktop/system notification**:
  - Optionally displays a system notification when opening the compose window to inform about the adjusted sender address.

### Email Accounts (Per-Account Activation/Deactivation)
- **Account List**: Lists all email accounts configured in Thunderbird (e.g., IMAP or POP3 accounts) with their name, account type, and associated identities.
- **Account Switch (Active / Inactive)**:
  - **Enabled (Default)**: When replying to emails for this account, the add-on intervenes if no matching identity exists and sets the original `TO` address as the sender (`FROM`).
  - **Inactive**: No automatic sender replacement is performed for this account; Thunderbird uses its default behavior.
- **Identity Preview (Collapsible)**: Displays the configured identities (`Name <address@domain.com>`) for each account that are evaluated during matching.

### Automatic Saving
- All changes to toggles and options take effect immediately and are saved automatically – no manual confirmation via a save button is required.

# Project Structure

```
thunderbird-auto-from/
├── manifest.json              # WebExtension manifest (Manifest V3)
├── package.json               # Project and test configuration
├── README.md                  # Documentation (English)
├── README.de.md               # Documentation (German)
├── LICENSE                    # MIT License
├── _locales/
│   ├── de/messages.json       # German localization
│   └── en/messages.json       # English localization
├── icons/                     # Extension icons (SVG, 16, 32, 48, 64, 128 px)
├── src/
│   ├── background/
│   │   ├── auto-from-handler.js # Core logic for matching & address assignment
│   │   └── background.js        # WebExtension background event listeners
│   ├── options/               # Options page (accounts & settings)
│   │   ├── options.html
│   │   ├── options.css
│   │   └── options.js
│   ├── popup/                 # Status popup in compose window
│   │   ├── popup.html
│   │   ├── popup.css
│   │   └── popup.js
│   └── utils/
│       ├── email-utils.js     # Email parser & address helper functions
│       ├── storage-utils.js   # Configuration & storage management
│       └── i18n-helper.js     # Localization helper
├── test/                      # Automated unit tests (Node.js test runner)
│   ├── email-utils.test.js
│   ├── storage-utils.test.js
│   ├── auto-from-handler.test.js
│   ├── options.test.js
│   ├── background.test.js
│   └── build.test.js
└── scripts/
    └── build.js               # Build script for .xpi and source ZIP archives
```

# Development & Testing

## Prerequisites
- Node.js (version 18+)

## Running Tests
```bash
npm test
```

## Syntax & Linter Check
```bash
# Runs both JavaScript syntax check and XPI validation with webext-linter
npm run lint

# Alternatively run only JavaScript syntax check
npm run lint:js

# Alternatively run only XPI validation with webext-linter
npm run lint:xpi
```

## Cleaning Build Artifacts
```bash
npm run clean
```

## Building Release Packages

> [!note]
>
> *The add-on's version number is maintained solely in `package.json` and is incorporated into the manifest during the build process.*

```bash
# Builds both the installable XPI and the source ZIP
npm run build

# Alternatively build only the unpacked add-on directory
npm run build:unpacked

# Alternatively build only the XPI package
npm run build:xpi

# Alternatively build only the source ZIP archive
npm run build:source
```
The generated release artifacts are stored in the `dist/` directory:
- `dist/itsc-auto-from/` (Unpacked extension for development and testing)
- `dist/itsc-auto-from-<version>.xpi` (Installable Thunderbird add-on)
- `dist/itsc-auto-from-<version>-source.zip` (Full source code for submission / archiving)

# Installation in Mozilla Thunderbird

1. Create the add-on package via `npm run build:xpi` (or `npm run build`).
2. Open Thunderbird.
3. Open the menu **Tools** (or hamburger menu) -> **Add-ons and Themes**.
4. Click the gear icon (top right) and select **Install Add-on From File...**.
5. Select the `dist/itsc-auto-from-<version>.xpi` file and confirm the installation.
6. For temporary development: Load the directory `dist/itsc-auto-from` as a temporary add-on under **Tools** -> **Developer Tools** -> **Debug Add-ons**.

# Issues and Feature Requests

- If you encounter bugs or want to request features, please open an issue in the GitHub repository `thunderbird-auto-from`:
  [https://github.com/ITSC-GmbH/itsc-auto-from](https://github.com/ITSC-GmbH/itsc-auto-from)
- Please check beforehand if an issue for the same topic already exists.
- When reporting bugs, please provide steps to reproduce; screenshots are helpful.

# Source Code Availability

The source code for each release is freely available; you can download it from the GitHub project page under the ["Releases"](https://github.com/ITSC-GmbH/itsc-auto-from/releases) section.

# License

This project is licensed under the **MIT License** – see the [LICENSE](LICENSE) file for details.

Copyright (c) 2026 ITSC IT Systems Consulting GmbH
