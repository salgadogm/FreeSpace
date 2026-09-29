FreeSpace
An integrated personal website project intended to improve daily productivity and financial documentation.
Project status: the repository currently contains a feature-rich React/TypeScript prototype and an in-progress migration to Laravel 13 + Livewire 4. The two stacks are not yet fully integrated end-to-end.

Overview
FreeSpace is designed as a personal workspace that combines financial management, productivity tracking, reading and learning tools, career preparation, and personal settings in one dashboard.
The current repository has two main application layers:
- React prototype at the repository root — the current visual and interaction source of truth.
- Laravel application under backend/ — the target modular-monolith implementation that is still being migrated from the React version.
The React application currently uses in-memory seed data for many screens, so the interface can still be explored when the external API is unavailable.
Main Features
Dashboard
- Personal workspace overview.
- Summary cards and visual charts.
- Shared top bar, notifications, profile controls, sharing dialog, and AI-assistant panel UI.
- Indonesian and English interface support.
- Light and dark theme handling.
Finance
- Transactions.
- Budget management.
- Analysis and projection.
- Financial condition overview.
- Investment tracking.
- Accounting workspace.
- Personal tax / SPT workspace.
Productivity
- Activity planning and tracking.
- Daily journal.
- Daily Bible / reflection page.
- Steps tracker.
Library & Learning
- Book collection management.
- PDF-oriented reading workspace.
- Accounting learning space.
Career
- CV Builder.
- ATS Analyzer.
- TOEFL training.
- Interview simulator.
System & Preferences
- Activity/history page.
- Profile page.
- Display and application preferences.
- Data import/export and backup-oriented UI.
- Notification settings.
- Security and data settings.
Technology Stack
Current React Prototype
Area	Technology
UI	React 18 + TypeScript
Build tool	Vite 6
Styling	Tailwind CSS 4
Components	Radix UI, MUI
Icons	Lucide React
Charts	Recharts
Forms	React Hook Form
HTTP client	Axios
Realtime adapter	Socket.IO Client
Spreadsheet utilities	SheetJS / xlsx
AI adapter	Google Generative AI SDK


Laravel Migration Target
The backend/ directory is structured toward the following stack:
Area	Technology
Runtime	PHP 8.3
Framework	Laravel 13
Interactive UI	Livewire 4 + Alpine.js 3
Styling	Tailwind CSS 4.1
Authentication	Laravel Fortify
Database	MariaDB / Eloquent
Charts	ApexCharts
Calendar	FullCalendar
Rich text	Tiptap
PDF reader	PDF.js
Files / activity	Spatie Media Library + Activity Log
AI providers	Gemini and Grok provider abstractions


Repository Structure
FreeSpace/
├── src/
│   ├── app/
│   │   ├── App.tsx                 # Main React shell and manual page navigation
│   │   ├── components/             # FreeSpace pages and reusable UI components
│   │   ├── hooks/                  # React hooks
│   │   └── services/               # API, auth, AI, storage, socket, Turnstile adapters
│   ├── imports/                    # Images and design/migration references
│   └── styles/                     # Global, Tailwind, font, and theme styles
├── backend/
│   ├── app/Modules/                # Laravel modular domains
│   │   ├── Accounting/
│   │   ├── AI/
│   │   ├── Career/
│   │   ├── Dashboard/
│   │   ├── Finance/
│   │   ├── Library/
│   │   ├── Productivity/
│   │   └── System/
│   ├── database/                   # Migrations and seeders
│   ├── resources/                  # Blade, CSS, and JavaScript
│   ├── routes/                     # Laravel routes
│   ├── tests/                      # Pest / PHPUnit tests
│   ├── MIGRATION_MATRIX.md         # React → Laravel migration status
│   └── DEPLOYMENT_HOSTINGER.md     # Intended deployment guide
├── package.json                    # React prototype dependencies/scripts
├── vite.config.ts
└── README.md
How the Current React Prototype Works
src/app/App.tsx keeps the current page in React state and renders the selected module directly instead of using URL-based routing.
src/app/components/DataStore.tsx starts with seeded local data and then attempts to hydrate selected datasets from an API. If that request fails, the local seed data remains active. The same store also contains Socket.IO listeners for realtime asset-related events.
The frontend service layer still reflects an older API architecture:
- VITE_API_URL defaults to http://localhost:3001/api.
- Service comments and endpoints refer to a NestJS backend and MinIO storage.
- The repository now contains a Laravel backend migration target instead.
Because of that transition, the React API adapters and the Laravel application should currently be treated as separate work-in-progress layers.
Run the React Prototype
Prerequisites
- A recent Node.js installation.
- npm, pnpm, or another compatible package manager.
Install and start
npm install
npm run dev
For a production build:
npm run build
Important note for this ZIP snapshot
src/app/App.tsx contains this unused import:
import asistenAiLogo from "../imports/Asisten_AI_Aset_SV_IPB.png";
The referenced image is not included in the provided ZIP. Restore the image at that path or remove the unused import before running/building the React application.
Optional Frontend Environment Variables
The current service layer recognizes these Vite variables:
VITE_API_URL=http://localhost:3001/api
VITE_GEMINI_API_KEY=your_key_here
VITE_TURNSTILE_SITE_KEY=your_site_key_here
VITE_API_URL belongs to the legacy API bridge. The current FreeSpace AI panel in App.tsx uses a simulated response, while src/app/services/aiService.ts contains a separate Gemini client adapter.
Do not expose production AI secrets in a public frontend bundle. The code itself already indicates that AI requests should ultimately be proxied through the server.

Laravel Migration Status
The migration matrix in backend/MIGRATION_MATRIX.md marks the shared layout/navigation, dashboard, and transaction page as completed migration items. Most other FreeSpace pages are still marked pending.
The backend domain layer already contains models and database migrations for areas such as:
- Finance and budgets.
- Accounting periods, chart of accounts, journals, and journal lines.
- Productivity activities, journals, reminders, and step tracking.
- Library books, bookmarks, highlights, notes, and reading sessions.
- Career CV documents/versions and interview questions.
- System notification preferences.
- Gemini/Grok AI provider abstractions.
Current backend snapshot limitations
The backend/ directory is not yet a complete runnable Laravel application in this ZIP snapshot. For example:
- bootstrap/app.php expects routes/api.php, but that file is not present.
- routes/web.php references many Livewire classes that have not been created yet.
- Only the Dashboard and Transactions Livewire classes are currently present.
- Routes reference controller classes that are not present in the snapshot.
- The Laravel CLI entry point is stored as backend/artisan/main.tsx instead of a root backend/artisan file.
- backend/vite.config.ts references resources/js/app.js, while the repository currently contains resources/js/app.ts.
- Several JavaScript modules imported by backend/resources/js/app.ts are still missing.
- .env.example is not included in the backend snapshot.
For that reason, backend/DEPLOYMENT_HOSTINGER.md should be treated as the intended deployment procedure after the Laravel migration is completed, not as a guarantee that this exact ZIP can already be deployed as-is.
Legacy Code Notice
The repository still includes several asset-management components and services from an earlier application context, including asset, maintenance, mutation, procurement, stock-opname, document, user, and activity-log functionality.
Some of this legacy code is still imported or used by the React data layer, while the current FreeSpace sidebar focuses on Finance, Productivity, Library, Career, History, and Settings. The migration plan explicitly identifies this legacy asset-management code as cleanup work after feature parity is achieved.
Recommended Next Steps
1. Fix the missing React image import so the current prototype can build cleanly.
2. Decide whether the React prototype remains a temporary reference only or continues as an active frontend.
3. Complete the missing Laravel application scaffold and Livewire pages listed in backend/MIGRATION_MATRIX.md.
4. Replace the legacy NestJS-oriented frontend API bridge with Laravel endpoints or remove it after the migration.
5. Move AI calls behind the server before production use.
6. Remove unused legacy asset-management code after Laravel feature parity is verified.
7. Add end-to-end tests for authentication, finance flows, productivity modules, data import/export, and backup/restore behavior.
Documentation Included in the Repository
- backend/MIGRATION_MATRIX.md — detailed React-to-Laravel migration mapping.
- backend/DEPLOYMENT_HOSTINGER.md — intended Hostinger shared-hosting deployment workflow.
- src/imports/pasted_text/freespace-migration-plan.md — migration/refactor requirements.
- src/imports/pasted_text/freespace-design-spec.md — design references.
- src/imports/pasted_text/productivity-module-design.md — productivity-module design notes.
- src/imports/pasted_text/career-module-design.md — career-module design notes.
- src/imports/pasted_text/library-module-design.md — library-module design notes.
FreeSpace is currently best understood as a working UI prototype plus an active Laravel migration codebase, with the React application serving as the reference for feature and visual parity during the migration.
