# CareGuide — Expo App

Native iOS/Android app built from the CareGuide web app.
Uses Expo Router (file-based navigation), Supabase (same backend), and the iOS-native design system.

---

## Setup (5 minutes)

### 1. Install Expo CLI
```bash
npm install -g expo-cli
```

### 2. Install dependencies
```bash
cd careguide
npm install
```

### 3. Start the dev server
```bash
npx expo start
```

Then press `i` for iOS simulator, `a` for Android emulator,
or scan the QR code with the **Expo Go** app on your phone.

---

## Project structure

```
careguide/
├── app/
│   ├── _layout.tsx          # Root layout — loads fonts, wraps AuthProvider
│   ├── index.tsx            # Entry — redirects to /auth/login or /tabs
│   ├── auth/
│   │   ├── _layout.tsx
│   │   ├── login.tsx        # Login screen
│   │   └── signup.tsx       # Signup with role selection
│   └── tabs/
│       ├── _layout.tsx      # Bottom tab navigator
│       ├── shift.tsx        # ☀️ Tasks / My Shift
│       ├── meds.tsx         # 💊 Medications
│       ├── clients.tsx      # 👤 Clients + detail view
│       ├── updates.tsx      # 📢 Announcements
│       └── dashboard.tsx    # ⚙️ Admin dashboard (admin/supervisor only)
├── lib/
│   └── supabase.ts          # Supabase client + shared types + helpers
├── hooks/
│   └── useAuth.ts           # Auth context (session, profile, signIn/Out)
├── constants/
│   └── theme.ts             # Colors, fonts, shadows, radii
├── app.json                 # Expo config
├── babel.config.js
└── tsconfig.json
```

---

## Key design decisions

| Web app | Expo app |
|---|---|
| `position: fixed` bottom nav | Native `<Tabs>` navigator — iOS handles this |
| `backdrop-filter: blur()` tab bar | `position: 'absolute'` on iOS gives native blur automatically |
| CSS `LinearGradient` | `expo-linear-gradient` |
| `document.getElementById` DOM manipulation | React state (`useState`, `useCallback`) |
| `localStorage` sidebar collapse | Not needed — no sidebar on mobile |
| Supabase JS SDK (CDN) | Same SDK via `npm` — identical API calls |
| `font-family: 'DM Sans'` | `@expo-google-fonts/dm-sans` — same fonts |

---

## Adding admin features

The admin management screens (clients CRUD, team management, wizard, templates)
are ready to be added as **Stack screens** pushed from the dashboard tab.

Example — add a "New Client" screen:
```
app/
└── tabs/
    └── new-client.tsx    # Push with router.push('/tabs/new-client')
```

All your existing Supabase query logic from the web app copies in verbatim.

---

## Building for production

```bash
# Install EAS CLI
npm install -g eas-cli

# Configure builds
eas build:configure

# Build for iOS (requires Apple Developer account)
eas build --platform ios

# Build for Android
eas build --platform android
```
