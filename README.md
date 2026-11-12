# APEXLedger Web App

A responsive website for **Ntuli Accountants & Associates** that mirrors the APEXLedger Android app's navy and gold branding. Clients request services, upload documents and message their accountant; staff and administrators manage the work. It uses the **same Firebase project** as the Android app, so both apps share the same data.

## Links
| | |
|---|---|
| **Deployed website** | http://apexleger.infinityfree.me/ |
| **Website repository (this repo)** | https://github.com/PreciousMoloi/Apexleger_Web |
| **Android app repository** | https://github.com/Luvuyo14/ApexLedger |
| **Video presentation** | [PASTE YOUR VIDEO LINK HERE](PASTE_YOUR_VIDEO_LINK_HERE) |

| | |
|---|---|
| **Authors** | Luvuyo Yizani ST10442920, Nyiko Matladi ST10450403, Precious Moloi ST10452719 |
| **Module** | Work Integrated Learning / XISD6329 |
| **Technology** | HTML, CSS, JavaScript (Firebase web SDK) |
| **Backend** | Firebase Authentication + Cloud Firestore (project `apexledger-fc1c9`) |

---

## Screenshots

| Login (email and Google SSO) | Client dashboard | Service requests |
|---|---|---|
| ![Login](docs/screenshots/web-login.png) | ![Client dashboard](docs/screenshots/web-client-dashboard.png) | ![Service requests](docs/screenshots/web-service-requests.png) |

| Documents | Messages | Notifications |
|---|---|---|
| ![Documents](docs/screenshots/web-documents.png) | ![Messages](docs/screenshots/web-messages.png) | ![Notifications](docs/screenshots/web-notifications.png) |

| Staff dashboard | Reports | Admin control panel |
|---|---|---|
| ![Staff dashboard](docs/screenshots/web-staff-dashboard.png) | ![Reports](docs/screenshots/web-reports.png) | ![Admin panel](docs/screenshots/web-admin-panel.png) |

---

## Features

- **Sign in** with email and password or **Google single sign-on**; client registration and password reset.
- **Account lock-out** after 5 failed sign-in attempts (15 minutes) and **automatic logout after 30 minutes of inactivity**.
- **Role-aware screens** for clients, accountants, managers and administrators.
- **Service requests** with ticket numbers such as `TAX-2026-0001`, priority, status and overdue chips.
- **Documents**: upload (PDF, images and office files up to 5 MB), preview, and staff review (approve, reject, under review) with notifications.
- **Messages**: threaded conversations between clients and accountants.
- **Notification centre**, notification preferences, change password and dark mode.
- **Staff dashboard**: assigned requests, open work, conversations, status updates and internal notes.
- **Reports**: status donut chart, workload by accountant, overdue tickets, CSV export and print / PDF.
- **Admin control panel**: add and manage staff and clients, assignment, activate / deactivate / delete, permissions, audit log and system health.

---

## Setup before first run

1. In the Firebase Console, open project **apexledger-fc1c9**.
2. Go to **Project settings → Your apps** and register a Firebase **Web app** if one does not already exist.
3. The Web App ID is already configured in `app.js`: `1:72147795409:web:5ac6dd4bce283fd99a74f0`. Confirm the remaining Firebase configuration matches project `apexledger-fc1c9`.
4. Enable **Authentication → Sign-in method → Email/Password** and **Google**. Add your website domain under **Authentication → Settings → Authorized domains** (needed for Google sign-in).
5. Enable **Firestore Database**.
6. Review `firestore.rules.example` against your deployed data and the app's workflows before deploying. Test it with a client, an accountant and an administrator account.
7. Serve this folder over HTTP, for example `python -m http.server 8080`, then open `http://localhost:8080`. For deployment, use Firebase Hosting (or any web host) and add the domain to the authorized domains.

---

## How it works

- Profiles are stored at `users/{uid}` (`full_name`, `role`, `phone`, `company`, `address`).
- Service requests are stored in `tickets` with `clientId`, service, priority, status and timestamps.
- **Documents:** metadata is saved in `documents`, and the file itself is saved in 700 KB pieces under `documents/{id}/chunks/{00000…}` (Firestore Bytes, maximum 5 MB per file). The Android app also stores files as Firestore chunks, so both apps use the same collection.
- Threads and replies are stored in `threads` and `threads/{threadId}/messages` with `clientId`, `accountantId`, `senderId`, `messageBody`, `sentTimestamp` and `lastMessagePreview`.
- Staff worklists and ticket status changes are filtered by account assignment. The administrator overview is only available to users whose `role` allows it.
- Other collections shared with the app: `notifications` and `audit_log`.

---

## Security and compatibility notes

- Client registration always assigns `role: client`. There is no public role selector.
- Users must not be able to change their own `role`, `assignedAccountantId` or accountant assignments. Privileged operations (creating staff accounts, routing) should use a trusted Cloud Function.
- Security rules must be tested with separate client, accountant and admin accounts. A query that does not constrain ownership or assignment may be denied even when a user can read individual documents.
- Deleting a user removes the profile record only. Remove the sign-in account in **Firebase Authentication** as well.
- Firebase configuration and rules are not deployed from this repository. Authentication, permissions and database compatibility must be tested in your own Firebase project.

---

## Project structure

```
Apexleger_Web/
├── README.md
├── docs/
│   └── screenshots/          Images used in this README
└── web/
    ├── index.html            Single-page shell
    ├── app.js                Authentication, tickets, documents, messages
    ├── features.js           Reports, admin panel, notifications, extras
    ├── styles.css            Navy and gold design, light and dark themes
    ├── assets/               Logo
    ├── firebase.json
    ├── firestore.rules.example
    ├── storage.rules.example
    ├── DEPLOYMENT_CHECKLIST.md
    └── README.md
```

---

## Related

The Android app (Kotlin) has its own README, database design and unit tests: https://github.com/Luvuyo14/ApexLedger

## Licence

Academic project
