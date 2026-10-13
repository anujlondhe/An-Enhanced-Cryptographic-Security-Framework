# ChainVault: Enhanced Cryptographic Security Framework for Hyperledger Blockchain

A fully animated, enterprise-grade web application implementing the cryptographic security framework from the paper: *"An Enhanced Cryptographic Security Framework for Hyperledger Blockchain"*.

## 🌟 Features
- **🔐 AES-256 Client-Side Encryption** — Files are encrypted before leaving your device
- **✍️ Ed25519 Digital Signatures (EdDSA)** — Authentication & non-repudiation
- **⛓️ Blockchain Audit Ledger** — Hyperledger-style immutable hash chain
- **☁️ Google Drive Storage** — Encrypted ciphertext stored in your personal Drive
- **🔍 Integrity Verification** — SHA-256 hash + signature check on every download
- **🎨 Fully Animated UI** — Framer Motion animations throughout

## 🛠️ Tech Stack
| Layer | Technology |
| :--- | :--- |
| **Framework** | React 18 + Vite |
| **Animations** | Framer Motion |
| **AES Encryption** | crypto-js (AES-256-CBC) |
| **Digital Signatures**| tweetnacl (Ed25519) |
| **Cloud Storage** | Google Drive API v3 |
| **Auth** | Google OAuth 2.0 (GIS) |
| **Routing** | React Router v6 |
| **Notifications** | react-hot-toast |

## 🚀 Quick Start

### 1. Install dependencies
```bash
npm install
```

### 2. Set up Google Cloud Console
1. Go to https://console.cloud.google.com
2. Create or select a project
3. Enable these APIs:
   - Google Drive API
   - Google Picker API (optional)
4. Go to **Credentials → Create Credentials → OAuth 2.0 Client ID**
   - Application type: Web application
   - Authorized JavaScript origins: `http://localhost:5173`
   - Authorized redirect URIs: `http://localhost:5173`
5. Go to **Credentials → Create Credentials → API Key**
   - Restrict to Drive API

### 3. Configure credentials
Edit `src/config/google.js`:
```javascript
export const GOOGLE_CONFIG = {
  CLIENT_ID: 'YOUR_CLIENT_ID.apps.googleusercontent.com',
  API_KEY: 'YOUR_API_KEY',
  // ...
}
```

### 4. Configure OAuth consent screen
1. In Google Cloud Console → OAuth consent screen
2. Add your Google account as a Test User (required for development)
3. Set scopes: `drive.file`, `profile`, `email`

### 5. Run the app
```bash
npm run dev
```
Open `http://localhost:5173`

## ⚙️ How It Works

### Upload Pipeline
1. **User File**
2. → AES-256-CBC Encrypt (client-side, random 256-bit key)
3. → SHA-256 Hash of ciphertext
4. → Ed25519 Sign the hash (with private key)
5. → Create Blockchain Ledger Record (with blockHash chain)
6. → Upload encrypted JSON to Google Drive

### Download & Verify Pipeline
1. **Google Drive (encrypted JSON)**
2. → Re-compute SHA-256 hash
3. → Verify Ed25519 signature (with public key)
4. → Compare hash to stored hash
5. → AES-256 Decrypt → Original File

### Blockchain Ledger
Each record stores:
- `blockId` — unique block identifier
- `blockHash` — SHA-256 of entire block contents
- `prevHash` — hash of previous block (chain linkage)
- `dataHash` — SHA-256 of encrypted file
- `signature` — Ed25519 signature
- `eddsaPublicKey` — verifier public key
- `timestamp`, `fileName`, `fileSize`, `uploader`

## 📂 Project Structure
```text
src/
├── config/
│   └── google.js          # Google API credentials
├── hooks/
│   ├── useAuth.js          # Auth context & session
│   └── useGoogleDrive.js   # Google Drive integration
├── utils/
│   └── crypto.js           # AES + EdDSA + SHA-256 + Blockchain
├── components/
│   └── Sidebar.jsx         # Navigation sidebar
├── pages/
│   ├── LoginPage.jsx       # Animated login with Google OAuth
│   ├── Dashboard.jsx       # Stats & live chain visualization
│   ├── UploadPage.jsx      # Encrypt + sign + upload pipeline
│   ├── FilesPage.jsx       # List, verify & decrypt files
│   ├── LedgerPage.jsx      # Blockchain audit trail
│   └── KeysPage.jsx        # AES + EdDSA key management
├── App.jsx                 # Router + layout
├── main.jsx                # Entry point
└── index.css               # Design system + animations
```

## 🔒 Security Notes
- **Key Storage:** Keys are stored in `localStorage` — export them from the Key Management page for backup.
- **Client-Side Isolation:** The AES key never leaves the browser. Google Drive only receives the ciphertext. The Ed25519 private key never leaves the browser.
- **Production Recommendations:** 
  - Use a proper KMS (e.g., HSM, cloud KMS) instead of `localStorage`.
  - Deploy the blockchain ledger to an actual Hyperledger Fabric network.

## 📖 Based On
**Chincholkar, A., Joshi, M., Shep, V., Gole, P., Londhe, A., & Mirgale, S. (2026).** *An Enhanced Cryptographic Security Framework for Hyperledger Blockchain.* 2nd International Conference on Computing, Communication and Green Engineering (CCGE), Pune, India.
