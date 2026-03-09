# FlatSync V2

React + Firebase app for a shared house with 7–8 users.

## Included in V2

- Firebase Auth login
- Firestore monthly database buckets
- Fixed expense categories: Rent, Khala, Electricity, Wi-fi, Meal
- Admin can add unlimited custom monthly expenses
- Admin can set per-person amount and due date
- Payment tracking per member
- Receipt upload to Firebase Storage
- Payment options with bank / bKash / direct pay metadata
- Presence updates for coming home / leaving
- Daily meal selection and meal assignment
- Notification feed stored per month

## Firestore structure

```text
houses/{houseId}
  members/{memberId}
  months/{YYYY-MM}
    expenses/{expenseId}
    presence/{memberId}
    meals/{YYYY-MM-DD}
    notifications/{notificationId}
```

## Required Firebase products

- Authentication
- Cloud Firestore
- Cloud Storage

Firebase's web setup uses the modular JavaScript SDK, and Firebase recommends that modular API for production web apps. citeturn0search4
Cloud Firestore is used here for app data and Firebase Storage is used for receipt files. Firebase's docs also note that new default Storage buckets created after October 30, 2024 use the `PROJECT_ID.firebasestorage.app` format. citeturn0search0turn0search2

## Local setup

1. Copy `.env.example` to `.env`
2. Fill in your Firebase web app config values
3. Run:

```bash
npm install
npm run dev
```

## Create member docs

For each user, create a Firestore doc at:

```text
houses/{VITE_DEFAULT_HOUSE_ID}/members/{uid}
```

Suggested fields:

```json
{
  "name": "Shihab Mahmood",
  "email": "user@example.com",
  "role": "admin"
}
```

Use `role: "admin"` for admins and `role: "member"` for regular users.

## Suggested security rules outline

- Only authenticated users can read house data
- Only house members can read/write their own payment, meal, and presence entries
- Only admins can change expense totals, due dates, reminder fields, and create custom expenses
- Only authenticated users can upload receipts for their own path

## Next upgrades

- Push notifications with Firebase Cloud Messaging
- Invite flow for onboarding 7–8 members
- Admin reminder scheduler with Cloud Functions
- Direct DESCO / SamOnline payment deep links if provider supports them

Firebase Cloud Messaging is the right Firebase product for web/mobile push notifications, and background message handling on web uses a messaging service worker. citeturn0search1turn0search16
