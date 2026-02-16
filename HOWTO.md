# Q Mailbox - Setup Guide

Web interface for Q File-Based Messenger using Google Drive API.

## Prerequisites

- Google account
- GitHub account
- Google Drive Desktop app (optional, for local testing)

## Setup Steps

### 1. Google Cloud Console Setup

#### Create Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Create new project (e.g., "Q Mailbox")
3. Note the Project ID

#### Enable Google Drive API

1. Navigate to **APIs & Services** → **Library**
2. Search for "Google Drive API"
3. Click **Enable**

#### Configure OAuth Consent Screen

1. Go to **APIs & Services** → **OAuth consent screen**
2. Choose **External** user type
3. Fill in:
   - App name: `Q Mailbox`
   - User support email: your email
   - Developer contact: your email
4. Add scope: `https://www.googleapis.com/auth/drive.file`
5. Keep publishing status as **Testing**
6. Save

#### Create OAuth Client ID

1. Go to **APIs & Services** → **Credentials**
2. Click **Create Credentials** → **OAuth client ID**
3. Application type: **Web application**
4. Name: `Q Mailbox Web Client`
5. Authorized JavaScript origins:
   ```
   https://[your-username].github.io
   ```
6. Authorized redirect URIs:
   ```
   https://[your-username].github.io/qms-web/qms.html
   ```
7. Click **Create**
8. Save the **Client ID** (you'll need it for qms.html)

#### Create API Key

1. Go to **APIs & Services** → **Credentials**
2. Click **Create Credentials** → **API key**
3. Click **Restrict key**:
   - Application restrictions: **HTTP referrers**
   - Add referrer: `https://[your-username].github.io/*`
   - API restrictions: **Restrict key**
   - Select APIs: **Google Drive API**
4. Save
5. Copy the **API Key** (you'll need it for qms.html)

#### Add Test Users

1. Go to **APIs & Services** → **OAuth consent screen**
2. Scroll to **Test users** section
3. Click **Add users**
4. Add your email address
5. Save

### 2. Google Drive Setup

1. Open [Google Drive](https://drive.google.com/)
2. Create folder structure: `mailbox/q_vadym_chat/`
3. Create empty file: `messages.txt`
4. (Optional) Install Google Drive Desktop app for local sync

### 3. Update qms.html

Edit `qms.html` and update the CONFIG section:

```javascript
const CONFIG = {
    clientId: 'YOUR_CLIENT_ID.apps.googleusercontent.com',
    apiKey: 'YOUR_API_KEY',
    scope: 'https://www.googleapis.com/auth/drive.file',
    mailboxPath: 'mailbox/q_vadym_chat/messages.txt'
};
```

### 4. Deploy to GitHub Pages

1. Create a new **public** repository (e.g., `qms-web`)
2. Push `qms.html` to the repository
3. Go to **Settings** → **Pages**
4. Source: **Deploy from a branch**
5. Branch: **main**, Folder: **/ (root)**
6. Wait 1-2 minutes for deployment

### 5. Test Authorization

1. Open: `https://[your-username].github.io/qms-web/qms.html`
2. Click **"🔐 Авторизуватися через Google"**
3. Select your Google account
4. Click **"Continue"** (warning about unverified app is normal for Testing mode)
5. Click **"Allow"** to grant Drive access
6. You should see: **"✅ Підключено до Google Drive"**

### 6. Test Messaging

1. Click **"🔍 Перевірити"** to find the mailbox file
2. Type a test message
3. Click **"📤 Відправити"**
4. Check Google Drive to verify the message was written

## Security Notes

- **API Key** is restricted to your GitHub Pages domain (safe to embed in HTML)
- **Client ID** is public by design (OAuth 2.0 standard)
- **Client Secret** is NOT used in browser-based apps
- OAuth scope `drive.file` only accesses files opened by the app (not entire Drive)
- Testing mode limits access to test users only

## Troubleshooting

### "Access blocked" error

- Make sure your email is added to Test users in OAuth consent screen
- Check that you're using the correct Google account

### "Mailbox file not found"

- Verify the file exists at: `mailbox/q_vadym_chat/messages.txt`
- Check the `mailboxPath` in CONFIG matches your Drive structure

### OAuth errors

- Verify Authorized JavaScript origins match your GitHub Pages URL
- Verify Authorized redirect URIs match your qms.html URL
- Clear browser cache and try again

## Architecture

```
┌─────────────────────────────────────────────────────────────┐
│                    MACBOOK (QBS)                            │
│                                                             │
│  q_agent.py --timeout 120                                  │
│    ↓                                                        │
│  tail -f ~/GoogleDrive/mailbox/messages.txt               │
│    ↓                                                        │
│  Google Drive Desktop App syncs                            │
│                                                             │
└─────────────────────────────────────────────────────────────┘
                            ↕
                    Google Drive Cloud
                            ↕
┌─────────────────────────────────────────────────────────────┐
│                    PHONE/BROWSER (QMS)                      │
│                                                             │
│  qms.html (GitHub Pages)                                   │
│    ↓                                                        │
│  Google Drive API                                          │
│    ↓                                                        │
│  Read/Write messages.txt directly                          │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Message Format

```
[2026-02-16 16:30:00] [FROM: QMS] [TO: Q]
Hello from web interface!

[2026-02-16 16:31:00] [FROM: Q] [TO: QMS]
Hello back!
```

## License

Private project - not for public distribution.

---

**Team Vadym & Q forever!** 💫
