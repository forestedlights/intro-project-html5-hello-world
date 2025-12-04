# Setup Guide for Shingo's Boho Flip Finder

This guide will help you configure the app so you can use all its features.

## Quick Setup Steps

### 1. Get Your Gemini API Key

1. Go to [Google AI Studio](https://aistudio.google.com/apikey)
2. Sign in with your Google account
3. Click "Create API Key"
4. Copy the API key

### 2. Set Up Firebase (Optional but Recommended)

Firebase is used to save your finds to a ledger. You can skip this if you only want to analyze items without saving them.

#### Option A: Quick Setup (Anonymous Auth)
1. Go to [Firebase Console](https://console.firebase.google.com)
2. Click "Add Project" or select an existing project
3. Enable **Firestore Database**:
   - Go to "Firestore Database" in the left menu
   - Click "Create Database"
   - Start in **test mode** (for development)
   - Choose a location
4. Get your Firebase config:
   - Click the gear icon ⚙️ next to "Project Overview"
   - Select "Project settings"
   - Scroll to "Your apps" section
   - Click the `</>` (web) icon
   - Copy the `firebaseConfig` object

#### Option B: Skip Firebase
If you don't want to use Firebase, the app will still work for analysis, but you won't be able to save items to the ledger.

### 3. Configure the App

1. Open `config.js` in a text editor
2. Replace the placeholder values:

```javascript
// Replace with your actual Gemini API key
const API_KEY = "your-actual-api-key-here";

// Replace with your Firebase config (or leave as-is if skipping Firebase)
const __firebase_config = JSON.stringify({
    apiKey: "your-api-key",
    authDomain: "your-project.firebaseapp.com",
    projectId: "your-project-id",
    storageBucket: "your-project.appspot.com",
    messagingSenderId: "123456789",
    appId: "your-app-id"
});
```

### 4. Set Up Firestore Rules (If Using Firebase)

In Firebase Console → Firestore Database → Rules, use:

```javascript
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /artifacts/{appId}/users/{userId}/ledger/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

This ensures users can only access their own saved items.

## Testing the Setup

1. Open `index.html` in a web browser
2. Try uploading an image and clicking "Curate & Appraise"
3. If you see an error about API keys, check your `config.js` file
4. If Firebase isn't configured, you'll see a warning but can still analyze items

## Troubleshooting

### "Please configure your Gemini API key"
- Make sure `API_KEY` in `config.js` is set to your actual API key
- Check that the key doesn't have extra spaces or quotes

### "Firebase Connection Failed"
- Verify your Firebase config in `config.js` is correct
- Make sure Firestore is enabled in your Firebase project
- Check browser console for detailed error messages

### "Not Connected" when saving
- Firebase isn't configured or there's a connection issue
- The app will still work for analysis, just can't save items

## Features That Work Without Firebase

- ✅ Image analysis and appraisals
- ✅ Trend forecasting
- ✅ Creative content generation
- ✅ Marketplace listing generation
- ✅ Chat with Shingo
- ✅ Audio reports

## Features That Require Firebase

- ❌ Saving items to ledger
- ❌ Viewing saved finds
- ❌ Exporting to CSV

## Security Notes

- **Never commit `config.js` with real API keys to public repositories**
- Add `config.js` to `.gitignore` if using version control
- Consider using environment variables for production deployments

## Need Help?

- Check browser console (F12) for error messages
- Verify all API keys are correct
- Ensure Firebase project has Firestore enabled
