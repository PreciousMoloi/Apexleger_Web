# APEXLedger Web App — Android-aligned build

A responsive, single-shell website built to mirror the Android app's navy/gold branding, dashboard, client service requests, documents, threaded messages and profile. Staff and administration screens are role-aware. It uses the same Firebase project and collection names as the Android app: `apexledger-fc1c9`, `users`, `tickets`, `documents`, `threads/{threadId}/messages`, `notifications`, and `audit_log`.

## Setup before first run

1. In Firebase Console, open project **apexledger-fc1c9**.
2. Go to **Project settings → Your apps**. Register a Firebase **Web app** if one does not already exist.
3. The Web App ID has already been configured in `app.js`: `1:72147795409:web:5ac6dd4bce283fd99a74f0`. Confirm the remaining Firebase configuration matches project `apexledger-fc1c9`.
4. Enable **Authentication → Email/Password**. Enable Firestore Database and Firebase Storage.
5. Review `firestore.rules.example` and `storage.rules.example` against your deployed data and app workflows before deploying. These are examples, not a guarantee that every existing Android feature is covered. Existing Android document uploads use Firestore chunk documents, while this web build saves files to Firestore in 700 KB chunks (`documents/{id}/chunks/{00000…}`, field `data` as Firestore Bytes, max 5 MB per file); both use the `documents` metadata collection but storage implementation differs.
6. Serve this folder over HTTP, for example `python -m http.server 8080`, then open `http://localhost:8080`. For deployment, use Firebase Hosting and add the hosted domain to Authentication authorized domains.

## Implemented workflows

- Email/password sign-in, client registration and password reset through Firebase Authentication.
- User profiles stored at `users/{uid}` using the Android app's `full_name`, `role`, `phone`, `company`, and `address` fields.
- Service requests stored in `tickets` with `clientId`, service, priority, status and timestamps.
- Document files uploaded to Firebase Storage; metadata saved in `documents`.
- Threads and replies stored in `threads` and `threads/{threadId}/messages` using Android-compatible `clientId`, `accountantId`, `senderId`, `messageBody`, `sentTimestamp`, and `lastMessagePreview` fields.
- Staff worklists and ticket status changes, filtered by account assignment; administrator overview is available only to roles already authorized in the `users` profile.

## Security and compatibility notes

- Client registration always assigns `role: client`. Do not add a public role selector.
- Do not allow users to change their own `role`, `assignedAccountantId`, or accountant assignments. Privileged operations should use a trusted Cloud Function.
- The browser app cannot securely implement privileged account creation or reliable accountant routing by itself. Assignments should be made by the same trusted routing/backend logic as the Android app.
- Security rules must be tested with separate client, accountant and admin test accounts. A query that does not constrain ownership/assignment may be denied even when a user can read individual matching documents.
- Android currently stores document bytes in Firestore chunks (`documents/{id}/chunks/...`) whereas this web app uses Storage objects. If the two apps must open every uploaded file cross-platform, unify document storage behind one shared implementation before production.
- Firebase configuration and rules cannot be deployed from this ZIP. Live authentication, permissions, Storage CORS/limits and database compatibility must be tested in your Firebase project.
