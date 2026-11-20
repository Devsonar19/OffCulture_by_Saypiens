# OffCulture by Saypiens

> **Bringing internet culture into physical spaces — a hyper-local two-sided marketplace for communities and cafes.**

---

## 🌐 What is OffCulture?

OffCulture is a **Shadow Marketplace** that bridges the gap between offline communities and local cafes with underutilized seating during non-peak hours. Book clubs, developer circles, music enthusiasts, and creative guilds can discover and reserve the perfect physical space — without upfront rental fees or complex coordination.

Cafes get **guaranteed group spending** with zero onboarding friction. Communities get **curated, group-friendly spaces** with the right vibe, capacity, and power points.

---

## ✨ Core Features (Phase 1 MVP)

| Feature | Description |
|---|---|
| 🔑 **Passwordless Auth** | Email OTP (6-digit) via Supabase GoTrue — no SMS/DLT fees |
| 🗺️ **Venue Discovery** | Hyper-local search by micro-region, group capacity, acoustic profile & plug points |
| 📋 **Slot Request Funnel** | Communities submit date, time, attendee count & community description |
| 🤖 **Proxy Booking Engine** | OffCulture operator manually confirms with cafe via Telegram alert → status flips live |
| ⚡ **Real-time Status Updates** | Instant `pending → confirmed / rejected` via Supabase Realtime WebSocket |

---

## 🏗️ Tech Stack

```
┌──────────────────────────────────────────────────────┐
│                   Flutter Client                      │
│           (BLoC • Supabase Realtime • Dart)          │
└──────────────────────┬───────────────────────────────┘
                       │ REST / JWT
┌──────────────────────▼───────────────────────────────┐
│              FastAPI Proxy Orchestrator               │
│       (Python 3.11+ • Pydantic • Uvicorn)            │
└──────────────────────┬───────────────────────────────┘
                       │ SQL / Realtime
┌──────────────────────▼───────────────────────────────┐
│          Supabase (PostgreSQL 15 + PostGIS)           │
│           Row Level Security • Auth • Realtime        │
└──────────────────────────────────────────────────────┘
```

| Layer | Technology |
|---|---|
| **Mobile App** | Flutter (Dart) · `flutter_bloc` · `supabase_flutter` |
| **API** | FastAPI (Python 3.11+) · Pydantic v2 · Uvicorn |
| **Database** | Supabase · PostgreSQL 15 + PostGIS (spatial queries) |
| **Auth** | Supabase Email OTP (passwordless) |
| **Notifications** | Telegram Bot (operator alerts for manual proxy bookings) |

---

## 📁 Project Structure

```
OffCulture_by_Saypiens/
├── .antigravity/
│   ├── rules.md          # Engineering & coding rules
│   └── memory.md         # Project context & decisions
├── docs/
│   ├── prd.md            # Product Requirements Document
│   ├── architecture.md   # Technical Architecture
│   ├── phases.md         # Roadmap & Execution Phases
│   └── design.md         # Design Tokens & UI System
├── backend/              # FastAPI Proxy Orchestrator
│   ├── main.py           # App entrypoint & routes
│   ├── schemas.py        # Pydantic request/response models
│   └── requirements.txt  # Python dependencies
├── mobile_app/           # Flutter Community App
│   ├── pubspec.yaml
│   └── lib/
│       ├── main.dart
│       ├── core/constants/colors.dart
│       └── features/
│           ├── auth/     # Email OTP auth flow
│           ├── discovery/# Venue search & filtering
│           └── booking/  # Slot request & status tracking
└── supabase/
    └── 001_initial_schema.sql  # Schema, RLS, PostGIS functions
```

---

## 🎨 Design Tokens

| Token | Hex | Usage |
|---|---|---|
| Background | `#1F2937` | Charcoal Slate — app background |
| Surface | `#4B3A2F` | Deep Espresso — cards & surfaces |
| Accent | `#B8860B` | Dark Goldenrod — primary CTA |
| Border | `#8B6B2E` | Antique Brass — dividers & borders |
| Foreground | `#EFE3C5` | Warm Cream — text & icons |

---

## 🚀 Roadmap

### Phase 1 — Shadow Marketplace *(Current)*
- ✅ Supabase schema + PostGIS + RLS policies
- ✅ FastAPI proxy with Telegram operator alerts
- ✅ Flutter BLoC app with Email OTP
- ✅ Real-time booking status updates
- 🔲 Seed 15 verified local cafes in Surat

### Phase 2 — Automated Venue Onboarding
- Flutter Web portal for cafe owners
- OffCulture Partner Android App with FCM push notifications
- Auto-accept / reject toggle (`booking_mode = 'auto_direct'`)
- Attendee check-ins & community feedback

### Phase 3 — Monetization & Expansion
- Razorpay / Cashfree payment integration (Minimum Spend Guarantees)
- Commission per confirmed head or Cafe Subscription Tier
- Multi-city expansion across Gujarat and Tier-1 hubs

---

## ⚙️ Getting Started

### Prerequisites
- Flutter SDK ≥ 3.2.0
- Python 3.11+
- A [Supabase](https://supabase.com) project with PostGIS enabled

### Backend (FastAPI)

```bash
cd backend
pip install -r requirements.txt
# Set environment variables
export SUPABASE_URL="https://your-project.supabase.co"
export SUPABASE_KEY="your-anon-key"
export TELEGRAM_BOT_TOKEN="your-bot-token"
export TELEGRAM_CHAT_ID="your-chat-id"
# Run the server
uvicorn main:app --reload
```

### Mobile App (Flutter)

```bash
cd mobile_app
flutter pub get
flutter run
```

### Database (Supabase)

Run `supabase/001_initial_schema.sql` in your Supabase SQL Editor to initialize:
- `profiles`, `cafes`, `bookings` tables
- PostGIS spatial index & `search_cafes` RPC function
- Row Level Security policies
- Auto-profile provisioning trigger on user signup

---

## 📐 Architecture Principles

- **Shadow Marketplace First** — `booking_mode = 'manual_proxy'` removes venue onboarding resistance in Phase 1.
- **No Over-Engineering** — Supabase Studio serves as the internal OffCulture control room.
- **Fail Fast on Spatial** — All geographic queries use native PostGIS (`ST_DWithin`, `ST_MakePoint`). No distance calculations in Dart or Python memory.
- **Zero Raw Secrets** — No Supabase Service Role keys or Telegram tokens in Flutter or git-tracked files.
- **Realtime-First** — Supabase Realtime WebSocket streams booking status changes directly to the Flutter client.

---

## 👥 Target Personas

**Community Organizer** — Discovers and locks in venue slots filtered by micro-region, group capacity (10–30 pax), vibe, and plug points. No upfront fees.

**Cafe Manager** — Gets guaranteed group spending commitments without adopting new software or complex onboarding.

---

## 🏢 Built by Saypiens

OffCulture is a product by **[Saypiens](https://github.com/Devsonar19)** — building culturally-aware, community-first digital products.

---

*Licensed under MIT. See [LICENSE](LICENSE) for details.*
