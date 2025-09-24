# Family To-Do List

A real-time shared to-do list application with Google authentication and secure access control.

## Features

- 🔐 **Google Authentication** - Secure login with Google accounts
- 👥 **User Management** - Share lists with specific users by email
- 🔒 **Access Control** - Only authorized users can view/edit lists
- ⚡ **Real-time Sync** - Changes appear instantly across all devices
- 📱 **Responsive Design** - Works on desktop and mobile
- 🏷️ **Categories** - Organize items with custom categories
- 📊 **Status Tracking** - Pending and completed items

## Setup Instructions

### 1. Firebase Configuration

1. **Create Firebase Project:**
   - Go to [Firebase Console](https://console.firebase.google.com)
   - Create a new project
   - Enable Authentication (Google provider)
   - Enable Realtime Database (test mode)

2. **Get Configuration:**
   - Project Settings → Your apps → Add web app
   - Copy the config object

3. **Update Config File:**
   ```bash
   # Copy the template
   cp config.js.template config.js
   
   # Edit config.js with your Firebase credentials
   nano config.js
   ```

### 2. Security Setup

**Important:** Never commit `config.js` to a public repository!

```bash
# Add to .gitignore (already included)
echo "config.js" >> .gitignore
```

### 3. Firebase Security Rules

Update your Realtime Database rules:

```json
{
  "rules": {
    "lists": {
      "$listId": {
        ".read": "auth != null && (data.child('sharedUsers').hasChild(auth.token.email) || data.child('sharedUsers').numChildren() == 0)",
        ".write": "auth != null && (data.child('sharedUsers').hasChild(auth.token.email) || data.child('sharedUsers').numChildren() == 0)",
        "sharedUsers": {
          ".read": "auth != null && (data.hasChild(auth.token.email) || data.numChildren() == 0)",
          ".write": "auth != null && (data.hasChild(auth.token.email) || data.numChildren() == 0)"
        }
      }
    }
  }
}
```

### 4. Deployment Options

#### Option A: GitHub Pages (Public)
```bash
# Copy to docs folder
cp shared-list/index.html docs/index.html
cp shared-list/config.js docs/config.js

# Commit and push
git add .
git commit -m "Deploy family to-do list"
git push
```

#### Option B: Netlify (Private)
1. Connect your GitHub repo to Netlify
2. Set build command: `echo "No build needed"`
3. Set publish directory: `shared-list`
4. Add environment variables for Firebase config

#### Option C: Vercel (Private)
1. Connect your GitHub repo to Vercel
2. Set root directory: `shared-list`
3. Add environment variables for Firebase config

## Usage

1. **First User (List Owner):**
   - Open the app and sign in with Google
   - Start adding items
   - Click "Share List" to add other users

2. **Additional Users:**
   - Get the app URL from the list owner
   - Sign in with Google
   - Access is automatically granted if you're in the shared users list

## Security Features

- ✅ **Authentication Required** - Must sign in with Google
- ✅ **Access Control** - Only shared users can access lists
- ✅ **Real-time Validation** - Access checked on every operation
- ✅ **Private Configuration** - Firebase config not in public repo
- ✅ **User Management** - Add/remove users from lists

## File Structure

```
shared-list/
├── index.html          # Main application
├── config.js           # Firebase config (private)
├── config.js.template  # Template for config
├── .gitignore          # Keeps config.js private
└── README.md           # This file
```

## Troubleshooting

**"You do not have access to this list"**
- Ask the list owner to add your email via "Share List"

**"Firebase not configured"**
- Make sure `config.js` exists and has valid Firebase credentials

**"Google sign-in failed"**
- Check that Google provider is enabled in Firebase Console
- Verify your domain is in authorized domains

## Development

For local development:
```bash
# Serve locally
python3 -m http.server 8000

# Open in browser
open http://localhost:8000/shared-list/
```

## License

Private use only. Do not distribute without permission..
