# Privacy Policy — Mythical

**Effective date:** 2026-06-23
**Last updated:** 2026-06-23

Mythical is a minimalist iOS calorie tracker, built and operated as a personal project by Vasyl Falach ("we", "us"). This page explains exactly what data the app touches, where it goes, and what choices you have. We keep the scope deliberately small because that's a load-bearing rule of the project — see the [Local-First Privacy](docs/wiki/principles/Local-First%20Privacy.md) principle.

## TL;DR

- **Photos stay on your phone.** They never leave your device.
- **Profile + meal totals sync to a database** (Supabase) so your history survives reinstalls. Only you can read them.
- **AI meal parsing** sends the meal description (and the photo if you used the photo flow) to a third-party AI provider — DeepSeek for text, OpenAI for vision and chat. We do not retain that input ourselves.
- **No analytics, no ad SDKs, no trackers.** We genuinely don't know how often you open the app.
- **Sign in with Apple is the only login method.** You can use Apple's private email relay if you want to keep your real address private.

## What we collect

### Identity (when you sign in)

- **Apple ID identifier** (or Apple's private-relay email if you opted into hide-my-email)
- Nothing else — no display name, no photo, no contacts

### Profile (when you onboard)

Stored on your device + synced to our database:

- Biological sex, age, height, weight
- (Optional) target weight, body-composition goal, weekly weight-change rate
- Activity level, goal type (lose / maintain / gain)
- Computed daily targets (calories, protein, carbs, fat, fiber, sugar)
- Your timezone (so the day rolls over correctly)

All of this is used solely to compute your nutrition targets via the [Goal Formula](docs/wiki/architecture/Goal%20Formula.md). We don't share any of it.

### Meals you log

Stored on your device + synced to our database:

- Meal description, item names, weights, quantities
- Calories and macros (protein, carbs, fat, fiber, sugar)
- The date you logged the meal
- Whether you entered it by photo, text, or manual

### Photos

**Photos never leave your device.** They are stored only in the app's local document folder and are automatically deleted after 90 days. They are not uploaded to our database, not backed up to iCloud unless your phone's iCloud Backup includes app data, and not shared with any third party.

### AI chat history (if you opt in)

The AI Chat tab is opt-in. If you enable it, your chat messages are stored **locally only** on your iPhone (90-day auto-prune). They are not uploaded to our database. Each chat message is sent to OpenAI to generate a response — see the third-party table below.

### What we do NOT collect

- Analytics — no SDKs of any kind in v1
- Crash reports beyond standard Apple TestFlight / App Store telemetry (which Apple handles, we receive aggregate stats only)
- Location, contacts, calendar, HealthKit, motion, or any other system-level data
- Device fingerprints, ad identifiers, or any tracking IDs

## Third-party providers

| Provider | What we send | Purpose |
|---|---|---|
| **Apple** (Sign in with Apple) | Your Apple ID credential | Sign-in |
| **Supabase** (Postgres + Auth, EU region) | Profile fields, meal aggregates (name, calories, macros, date) | Sync across devices and reinstalls |
| **OpenAI** | Meal description + the photo you took, when you use the photo flow; your chat message + a privacy-minimized snapshot of your recent meals, when you use AI chat | AI estimation of nutrition; grounded AI chat answers |
| **DeepSeek** | Meal description text (no photos) when you use the text flow | AI estimation of nutrition |

Each provider receives only what's needed for the immediate request. We do not send your full meal history to OpenAI or DeepSeek. The chat-context snapshot sent to OpenAI is privacy-minimized — it includes names, dates, calories, and macros, never photos or raw rows. See the [AI Chat Privacy Boundary](docs/wiki/decisions/AI%20Chat%20Privacy%20Boundary.md) decision for the exact contract.

Each provider has their own privacy policy: [Supabase](https://supabase.com/privacy), [OpenAI](https://openai.com/policies/privacy-policy), [DeepSeek](https://platform.deepseek.com/privacy), [Apple](https://www.apple.com/legal/privacy/).

## How we secure your data

- All network traffic uses HTTPS.
- API keys for AI providers are stored in the iOS Keychain on your device.
- Database access is gated by Row-Level Security: a signed-in user can only read and write their own rows.
- We do not have third-party staff with access to your data; this is a single-developer project.

## Data retention

- **Account + profile + meal aggregates** (database): kept until you ask us to delete them
- **Photos** (local only): auto-deleted after 90 days
- **AI chat history** (local only): auto-pruned after 90 days
- **Account deletion**: email us (see Contact) and we'll wipe everything within 7 days

## Your rights

- **See what we have**: the app is your view of it — every meal, every macro, every profile value is visible to you in-app
- **Edit anything**: tap any meal to change it; tap Profile → Edit to change targets and personal details
- **Delete a meal**: swipe-to-delete on any meal row, or use the Delete action inside meal detail
- **Delete everything**: sign out clears all local data; emailing us deletes your remote data
- **Export** (CSV of your meals): not in the app yet — email us and we'll send you a dump

## Children

Mythical is rated 4+ but is built for general use and is not directed at children under 13. We don't knowingly collect data from anyone under 13. If you believe a minor has signed up, email us and we'll remove the account.

## Changes to this policy

If we materially change how we handle your data, we'll update this page and bump the "Last updated" date. For TestFlight builds, material changes are also called out in the build's "What to Test" notes.

## Contact

**Vasyl Falach** — falachvasyl@gmail.com

For privacy questions, account deletion, or data export, this is the only address that reaches a human (me).
