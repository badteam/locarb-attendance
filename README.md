# LoCarb Attendance – Full v1 (Skeleton)

**Web + Mobile (Flutter)**
- Username/Password auth (email alias @locarb.app)
- Onboarding + Login/Signup
- Admin dashboard (users/branches/leaves/payroll)
- Employee home
- Attendance (check-in/out + location placeholder)
- Leaves admin (approve/reject, deduct balance)
- Payroll with visibility toggle
- FCM token save (web push via VAPID)
- Firebase Hosting CI (GitHub Actions)

> This is a functional skeleton to get you live fast. We’ll iterate features.

## After upload
1. In GitHub repo add secret **FIREBASE_SERVICE_ACCOUNT** (JSON key).
2. Commit to `main` → Actions builds & deploys to Firebase Hosting.
3. In Firestore → publish rules (see `FIRESTORE_RULES.md`).

## Notes
- If Firebase Storage fails with the provided storageBucket domain,
  change it to `<project-id>.appspot.com` in:
  - `lib/config/firebase_options.dart`
  - `web/firebase-messaging-sw.js`
