# JARVIS Android Assistant (starter project)

A native Kotlin Android starter with a sci-fi interface, speech-to-text, spoken replies, a basic “Hey Jarvis” phrase detector, a few local commands, and an optional AI backend.

## Important limitations
- This is **source code**, not a prebuilt APK. Build the APK in Android Studio using the steps below.
- Android's built-in `SpeechRecognizer` is not a dedicated always-on wake-word engine. This starter listens while the app is open and may pause/restart recognition depending on the device. It is not guaranteed to detect “Hey Jarvis” from the background or while the screen is off.
- Speech recognition generally uses the device's recognition service and may require internet. English (India) is configured initially; change the locale/recognition strategy for better Tamil recognition on your device.
- The AI server is optional but required for intelligent AI answers. Do not put your API key in the APK.
- Android will ask for microphone permission. The app does not secretly record audio; use the visible Start/Stop control.

## Build the Android APK
1. Install Android Studio on a computer: https://developer.android.com/studio
2. Open this `JarvisAndroid` folder in Android Studio.
3. Allow Gradle sync and install Android SDK Platform 35 if prompted.
4. Connect an Android phone with USB debugging enabled, or use an emulator.
5. Select **Build > Build Bundle(s) / APK(s) > Build APK(s)**.
6. Android Studio will show the generated APK location, normally `app/build/outputs/apk/debug/app-debug.apk`.
7. Copy that APK to your phone and install it. If Android blocks it, allow installation from that file manager/source.

## Configure the AI backend
1. Install Node.js 20+ on a computer/server.
2. In a terminal, open the `backend` folder and run `npm install`.
3. Set your API key as an environment variable (do not share it or commit it):
   - macOS/Linux: `export OPENAI_API_KEY="your-key"`
   - Windows PowerShell: `$env:OPENAI_API_KEY="your-key"`
4. Start it: `npm start`
5. Deploy this backend to a server with HTTPS. Do not expose a local development server publicly without authentication/rate limits.
6. In `app/src/main/java/com/example/jarvis/MainActivity.kt`, replace `https://YOUR_BACKEND_DOMAIN.example/chat` with your deployed HTTPS `/chat` endpoint.
7. Rebuild the APK.

## Tamil + English
The assistant replies in the language used when the backend is connected. The starter's Android speech recognizer is set to `en-IN` to provide a working default. For Tamil speech recognition, test changing `RecognizerIntent.EXTRA_LANGUAGE` to `ta-IN`; support depends on the installed speech service. A robust bilingual version should offer a language selector and test both locales on the target phone.

## Current built-in commands
- “Hey Jarvis” — wake phrase while the app is open
- After wake phrase: “What time is it?”
- After wake phrase: “Open YouTube”
- After wake phrase: “Open settings”
- After wake phrase: ask an AI question (once backend configured)
- “Stop listening” — stop voice listening

## Security and production checklist
Before public release, add backend authentication, rate limits, request-size limits, HTTPS, privacy disclosure, and clear recording indicators. Do not hardcode API keys in the Android app. For reliable background wake-word activation, implement a dedicated on-device hotword engine and a foreground service with a persistent notification, then test battery use and Android version restrictions.
