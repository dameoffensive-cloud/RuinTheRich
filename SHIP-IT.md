# RUIN THE RICH — how to ship it

You have 4 app files: `index.html`, `manifest.webmanifest`, `sw.js`, `icon.svg`.
Put them together in one folder (e.g. `ruin-the-rich/`). That folder IS the app.

---

## Stage 1: Put it online (do this first, ~5 min)

Everything else builds on having a URL.

1. Go to **app.netlify.com/drop** (free account).
2. Drag the folder onto the page.
3. You get a URL like `something.netlify.app` (rename it in Site settings).

That's it. The PWA stuff activates automatically once it's served over HTTPS:
visitors on phones get "Add to Home Screen," desktop Chrome/Edge shows an
install icon in the address bar, it works offline, and saves persist.

Test it: open the URL on your phone → share menu → Add to Home Screen.
It should launch fullscreen with the broke-billionaire icon.

## Stage 2: Google Play (~$25 once, doable on your Windows PC)

The modern lazy path is **Bubblewrap / TWA** (wraps your live URL) or
**Capacitor** (bundles the files into the app). Capacitor is more standard:

```
npm install -g @capacitor/cli
mkdir rtr-app && cd rtr-app
npm init -y
npm install @capacitor/core @capacitor/android
npx cap init "Ruin The Rich" com.yourname.ruintherich --web-dir=www
mkdir www          (copy the 4 app files into www/)
npx cap add android
npx cap open android
```

Then in Android Studio (free download): Build → Generate Signed App Bundle.
Upload the `.aab` to **play.google.com/console** ($25 one-time).

You'll also need:
- **App icons**: convert `icon.svg` to a 1024x1024 PNG (any svg-to-png site),
  save as `assets/icon.png`, then `npx @capacitor/assets generate`.
- **Screenshots**: 2+ phone screenshots (just screenshot the game).
- **Privacy policy URL**: required even though the game collects nothing.
  A one-line page on your Netlify site works: "This app collects no data.
  All saves are stored on your device."
- **Content rating questionnaire**: it's a casual game, no gambling with real
  money, no user content — sails through.

Review usually takes a few days. New personal accounts need 12+ testers for
2 weeks before public release (Google's rule since 2023 — check current
requirements when you get there).

## Stage 3: Apple App Store ($99/year, requires a Mac)

Same Capacitor project: `npm install @capacitor/ios && npx cap add ios`,
then `npx cap open ios` on a Mac with Xcode. Sign up at developer.apple.com.
Apple reviews web-wrapped apps more skeptically — the offline play, saves,
and native feel help your case. Honestly: skip until Android proves people
want it, or borrow a friend's Mac for an afternoon.

## Notes

- The game saves automatically (localStorage) and simulates up to 1000
  missed rounds when you come back. No accounts, no server, no costs.
- To update the app: edit `index.html`, bump `rtr-v1` to `rtr-v2` in `sw.js`
  (forces the cache to refresh), re-drag to Netlify. Store versions need a
  rebuild + new upload.
- If you ever add analytics or ads, the privacy policy and store forms need
  to reflect it.
