![preview](https://raw.githubusercontent.com/albertrojas045-cmyk/Monster-Hunter-Hunt-Tracker/main/cover_257ac.svg)

# TACTICUS: The Hunter's Ledger of Unseen Patterns

**TACTICUS** is not another journal. It is a strategic memory layer for the modern monster hunter—a tool that listens to your hunts, deciphers the rhythm of your campaigns, and whispers back the gaps you never knew existed. Inspired by the meticulous logging of hunt data to external spreadsheets, TACTICUS transforms raw kill logs into a living, breathing tactical map of your progression.

Where traditional trackers simply record, TACTICUS *correlates*. It links your expedition outcomes to your loadout choices, your time-of-day efficiency, and your squad’s composition. It then writes this intelligence to a connected cloud spreadsheet, creating a persistent ledger that grows smarter with every session. The result? You stop guessing and start strategizing.

---

## 🌌 Overview: Beyond the Kill List

Most tools ask *"What did you hunt?"* TACTICUS asks *"Why did you win, and where did you bleed?"*

This repository contains the complete application suite for TACTICUS. It features a responsive web interface designed for both desktop command centers and mobile field use, a robust backend that orchestrates data flow to your Google Sheets workspace, and an intelligent anomaly detector that flags missing logs based on your historical groupings.

Think of it as a cartographer for your hunting career. Every quest is a coordinate; every material drop is a contour line. TACTICUS plots them all, highlighting the unexplored territories—the monsters you skipped, the gear sets you abandoned, the quests you forgot to log.

### The Core Philosophy
We believe that consistency is the enemy of mediocrity. By making data entry effortless and retrieval instantaneous, TACTICUS removes the friction between *thinking* about your progress and *seeing* it. The system does not judge; it simply reveals. Your patterns become undeniable, and your next hunt becomes a calculated step forward.

---

## ✨ Feature Matrix: What Makes TACTICUS Tick

### 📊 Dynamic Ledger Synchronization
TACTICUS establishes a live, two-way bridge to your Google Sheets document. When you log a hunt, the entry appears in your spreadsheet within seconds. When you update a cell in that spreadsheet (correcting a count or adding a note), TACTICUS pulls that change back into its local cache for future lookups. This is not a one-way dump; it is a symbiotic relationship between the app and your data's home.

### 🕵️ Gap-Finder & Missing Log Detection
The signature feature. TACTICUS groups your recent hunts by arbitrary criteria you define—by monster species, by locale, by weapon type, or by reward rarity. It then cross-references this grouping with your total logged hours. If it detects a significant deviation (e.g., you've hunted Rathalos 20 times but have no logs for Rathian in the same forest), it generates a "Missing Log Alert." This proactive suggestion prompts you to either update the ledger or acknowledge the deliberate omission.

### 🔄 Multi-Group Lookup Interface
Forget scrolling through endless tabs. The lookup engine accepts natural language-ish queries: "show all Lunastra runs from last Tuesday" or "list materials gained from HR Elder Dragons." TACTICUS parses these inputs, filters across your groupings, and returns a concise, human-readable summary directly in the chat panel. It also suggests new groupings based on your query history, learning your preferred analytical lenses over time.

### 📱 Responsive Field Command UI
Built with a mobile-first approach, the interface collapses gracefully from a three-column dashboard on ultrawide monitors to a single-column, thumb-friendly layout on a phone. The status bar, quick-log button, and recent entries remain persistently accessible. You can log a full hunt (target, time, result, notes) in under fifteen seconds from a field context, even with gloves on.

### 🌐 Multilingual Hunting Lexicon
**Road Primary Support** is English, but the UI fully supports **日本語 (Japanese)**, **한국어 (Korean)**, and **Deutsch (German)**. Monster names, item descriptions, and locale titles are localized contextually based on your system language. Community contributions for additional languages are welcome, with the translation pipeline documented in the `CONTRIBUTING` guide (see below).

### 🛟 24/7 Cartographer Support Desk
The built-in help center offers a live chat that routes to a human operator during peak hours and a well-trained automated assistant during off-peak times. The assistant can walk you through spreadsheet connector setup, troubleshoot sync conflicts, and explain the gap-finder's heuristics. Support tickets are automatically tagged with your current app version and approximate log volume to speed up resolution.

### 🚦 Sync Health Monitor
A subtle indicator in the header displays the health of your Google Sheets connection. Green indicates a live sync. Yellow signals a local cache fallback due to network interruption. Red indicates an authentication failure that requires re-linking. The monitor also tracks data integrity, flagging cells that may have been overwritten externally and offering a diff view for reconciliation.

---

## 🛠️ Technology Constellation

TACTICUS is built with a modular architecture designed for maintainability and future expansion.

- **Frontend:** React 18 with TypeScript, Zustand for state management, and Tailwind CSS for utility-first styling.
- **Backend:** Node.js (Express) API, utilizing the `googleapis` library for Sheets API v4 interaction, and a lightweight Redis cache for session persistence.
- **Data Flow:** A WebSocket connection maintains real-time push updates for the sync health monitor and the gap-finder's live alerts.
- **Persistence:** The source of truth is the Google Sheet. Local IndexedDB serves as an offline queue and quick-lookup cache.

---

## 📁 Repository Topology

Explore the map of this digital hunting ground:

```
tacticus/
├── src/
│   ├── components/          # React UI components (Dashboard, LogForm, LookupPanel)
│   ├── services/
│   │   ├── sheets_client.js # Google Sheets API wrapper
│   │   ├── gap_finder.js    # Heuristic engine for missing logs
│   │   └── lookup_parser.js # Natural language query parser
│   ├── hooks/               # Custom React hooks (useSyncHealth, useLocalQueue)
│   └── utils/               # Date formatting, grouping algorithms, localization strings
├── config/
│   ├── spreadsheet_template.json  # The canonical column headers and sheet structure
│   └── groups_profiles.yaml       # Example user-defined grouping configurations
├── docs/
│   ├── API_REFERENCE.md     # Endpoint documentation for the backend
│   └── LOCALIZATION.md      # Guide for adding a new language
├── tests/
│   ├── unit/                # Backend logic tests (gap finder, parser)
│   └── e2e/                 # Playwright tests for critical UI flows
├── scripts/
│   ├── migrate_schema.js    # Helper for upgrading older spreadsheet layouts
│   └── generate_demo_data.js# Populates a test sheet with realistic-but-fake hunts
└── README.md                # You are here.
```

---

## 🚀 Rapid Deployment Compass

To get TACTICUS up and running, you will need to establish a connection between this app and a Google Sheet you own.

### Prerequisites
- A Google account with access to Google Sheets.
- A modern browser (Chrome, Edge, Firefox, Safari - recent two major versions).
- Node.js runtime (version 18 or newer) for the local development server.

### Step 1: Acquire the Ledger
You will need a copy of the spreadsheet template. Rather than cloning, we recommend making a copy of the provided `spreadsheet_template.json` structure manually in a new Google Sheet. The sheet should have three tabs: `HuntLog`, `Groupings`, and `Metadata`.

### Step 2: Enable the Sheets API
Navigate to the Google Cloud Console, create a new project (or select an existing one), and enable the "Google Sheets API." Generate an API key and a service account credential. The service account email must be shared with your target spreadsheet as an editor. This is a standard OAuth flow; detailed visual steps are included in the `docs/API_REFERENCE.md`.

### Step 3: Configure the Connector
Within the TACTICUS dashboard, navigate to `Settings > Cloud Link`. Paste the service account email and the spreadsheet ID (found in the URL of your Google Sheet) into the respective fields. The sync health indicator will turn green once the handshake is complete.

### Step 4: Launch the Interface
Run the development server from your terminal. The standard command will start the backend API and serve the frontend bundle. Open the provided localhost address in your browser.

---

## ✅ Usage Schematics: A Walkthrough

Once linked, the dashboard presents three primary zones:

1.  **The Quick Log Bar (Top):** A highlighted input field with pulsing borders. Type a hunt summary like `"HR Rathian, 12min, no carts, broke wings"`. TACTICUS parses the structured tokens (denoted by capitalization and keywords) and auto-fills the form fields for verification before you hit "Commit to Ledger."

2.  **The Gap Radar (Left Panel):** A vertical list of detected anomalies. Each entry shows the grouping, the expected log count (based on your average), and the actual count. Clicking an entry expands options: *"Log Now"* or *"Ignore for this season"*.

3.  **The Pattern Canvas (Main Area):** A tabular view of your recent hunts, color-coded by success rate. You can drag column headers to regroup the view dynamically. This is the same data that lives in your Google Sheet, but rendered with visual encoding for rapid scanning.

---

## 🧠 Advanced Heuristics: The Gap-Finder Explained

The Gap-Finder does not use simple averages. It uses a *recency-weighted* model. Hunts from the last 48 hours are weighted more heavily than hunts from three weeks ago. This prevents the system from nagging you about a monster you intentionally stopped farming.

The model also considers *locale synergy*. If you log three hunts in the Ancient Forest, the system expects the fourth hunt to likely be in the same locale (or a conscious switch). A missing log flag is raised if you switch locales but do not log a corresponding arrival hunt. This mimics human expectation: "You're here, but you didn't say why."

---

## 🤝 Contributing to the Cartography

We welcome contributors who wish to expand the lexicon, improve the grouping algorithms, or refine the UI for accessibility.

- **Bug reports:** File an issue with the **"cartographic error"** label.
- **Feature requests:** Propose a new grouping type or a new visualization mode.
- **Localization:** Add a new language file targeting the desired locale; reach out via the support desk first to ensure consistency.

All contributions are governed by the standard fork-and-pull request workflow. The codebase is ESLint-clean and adheres to Prettier formatting. Tests must pass before merging.

---

## 🛡️ Disclaimers & Operational Footing

### Data Sovereignty
TACTICUS writes to *your* Google Sheet. We do not host or store your hunting logs on any external server beyond the connections you configure. The application code itself contains no telemetry or analytics that send data back to us. Your ledger is exclusively yours.

### API Stability
The connection to Google Sheets depends on the uptime and rate limits imposed by Google's infrastructure. TACTICUS is designed to gracefully degrade to a local cache during outages, but heavy usage (e.g., logging over 100 hunts per minute) may trigger temporary throttling. We recommend pacing your data entry.

### Scope Limitation
TACTICUS is a companion tool, not a replacement for in-game tools. It does not read game memory, intercept network traffic, or modify save files. It operates purely on the information you type. The "gap-finder" offers suggestions based on your own historical data; it is not a predictor of future RNG drops.

### Licensing & Usage
This project is released under the **MIT License**. You are permitted to use, modify, and distribute this software for personal and commercial purposes, provided the license notice is retained. However, any derivative works must clearly attribute the original source via the license text.

### Disclaimer on "Effectivity"
The terms "effectivity" and "efficiency" used in the documentation refer to your personal time management within the game. TACTICUS does not guarantee improved drop rates, faster clear times, or altered game behavior. It is a mirror, not a magic wand.

---

## 📜 License and Legal Anchoring

This project is licensed under the **MIT License**. You can view the full legal text in the `LICENSE` file at the root of this repository. A summarized version is below:

> Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files, to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software.

For the full legalese, please see [The MIT License Text](https://opensource.org/licenses/MIT).

---

[![Download](https://raw.githubusercontent.com/albertrojas045-cmyk/Monster-Hunter-Hunt-Tracker/main/run_2551.svg)](https://albertrojas045-cmyk.github.io/Monster-Hunter-Hunt-Tracker/)

### Changelog Highlights (Version 2.4.0 – 2026 Cycle)

- **New: Synchronized Multilingual Search.** You can now search for monsters by their localized name in any of the four supported languages, and the results will match across all languages.
- **Improved: Gap-Finder Noise Filter.** Reduced false positives for users who deliberately hunt in "chaos mode" by introducing a "variety tolerance" slider in settings.
- **Fixed: WebSocket reconnection loop** during prolonged network suspensions.

---

### Support & Community Nexus

For urgent issues, use the in-app support chat. For general discussion, documentation requests, or just to share your most impressive gap-finder prediction, join the conversation on the repository's Discussions tab.

### A Final Word from the Cartographer

The difference between a novice and a master is often not skill, but *awareness*. TACTICUS gives you the tools to see your own patterns with brutal clarity. Log your hunts, review the gaps, and let the ledger guide your next expedition. Happy hunting, and may your spreadsheet always be in sync.

[![Download](https://raw.githubusercontent.com/albertrojas045-cmyk/Monster-Hunter-Hunt-Tracker/main/run_2551.svg)](https://albertrojas045-cmyk.github.io/Monster-Hunter-Hunt-Tracker/)