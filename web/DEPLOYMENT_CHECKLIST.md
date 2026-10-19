# Go-live checklist

- [x] Configure the Web App ID in `app.js`: `1:72147795409:web:5ac6dd4bce283fd99a74f0`.
- [ ] Enable Email/Password authentication and authorize localhost/production domains.
- [ ] Confirm Firestore and Storage are enabled and billing/quotas fit your use.
- [ ] Review and test security rules in Firebase Emulator Suite with a client, accountant and admin test account before deploying.
- [ ] Confirm staff users' `users/{uid}.role` values and client `assignedAccountantId` values are correct.
- [ ] Deploy trusted backend logic for accountant assignment, staff approval, admin account creation, and audit logging.
- [ ] Test new registration, login, reset password, create request, status update, upload/download file, create thread, reply, sign out, and denied cross-client access.
- [ ] Decide on one cross-platform document storage implementation. The current Android app writes document bytes as Firestore chunks; this web app writes file bytes to Firebase Storage.
- [ ] Confirm the deployed rules permit the exact queries each role uses. Never launch with open test rules.
