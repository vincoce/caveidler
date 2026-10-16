# Dripstone

A very slow idle game. Water drips, calcite builds up, and the cave grows on its own. Players in the same group can send each other bats and deer.

The game works without any setup as a single-player game. For bats and deer between players, connect it to a free Firebase project (about 10 minutes).

## 1. Create the Firebase project

1. Go to https://console.firebase.google.com and click **Create a project**. Google Analytics is not needed.
2. Open **Authentication → Get started → Sign-in method**. Click **Add new provider → Google**, enable it, choose a support email and save. Google is the only provider the game uses, so leave Anonymous (and everything else) turned off.
3. **Build → Firestore Database → Create database**. Pick a location near you (for example `europe-west3`, Frankfurt) and start in **production mode**.
4. In Firestore, open the **Rules** tab, replace everything with the contents of `firestore.rules`, and click **Publish**.
5. **Project settings (gear icon) → General → Your apps**, click the web icon `</>`, give it any name, and register it. Hosting is not needed.
6. Firebase shows a `firebaseConfig` object. Copy the values of `apiKey`, `authDomain`, `projectId` and `appId` into `firebase-config.js`.

The config values are meant to be public; the rules are what protect the data.

## 2. Put it on GitHub Pages

1. Create a new **public** repository on GitHub (GitHub Pages on private repos needs a paid plan).
2. Upload `index.html` and `firebase-config.js` (and this README if you like). When a new version of the game comes out, replace only `index.html`.
3. **Settings → Pages → Build and deployment**: Source **Deploy from a branch**, branch `main`, folder `/ (root)`. Save.
4. After a minute the game is live at `https://<your-username>.github.io/<repo-name>/`.
5. Back in Firebase: **Authentication → Settings → Authorized domains**, add `<your-username>.github.io`.

## 3. Play together

Open the page, click **Sign in with Google**, make up a group code (at least 6 characters, e.g. `cave-7392`), and enter your name. Send the link and the same code to the other player. Each of you gets your own cave and appears under **Other caves** for the other.

## Features that use the server

- **Groups** have an owner (whoever created the group), an optional password, and a public name that the owner can change once a week. The code itself never appears in the rankings.
- **Join requests:** anyone can ask to join a group from the Groups tab in Rankings (no code needed), or after typing a code they don't have the password for. The owner accepts or declines. The owner can hide the group from the Groups tab, and can switch on "New members need my approval" so the code or password alone isn't enough.
- **Group size:** at most 10 players. The owner can set a lower limit (2 to 10). The server keeps count, so the limit can't be passed.
- **Ownership:** the owner can make someone else the owner at any time. An owner who wants to leave while others are still in the group has to hand it over first. An owner who is alone can just leave; the group waits empty and they're still its owner if they come back.
- **Names** belong to the account and can change once a week (the first change is free). The server checks that the name shown in groups and rankings matches the account.
- **Rankings** list everyone who plays, by all-time calcite. Anyone can hide themselves with the checkbox above the list.
- **Visitor waiting times** are checked by the server, so they can't be skipped by editing the page.
- **The group project** only accepts additions: each member can only add to their own share.

The very first time you open the rankings, Firestore might ask for an index in the browser console. Single-field indexes are created automatically, so normally nothing is needed.

## Good to know

- Online play needs a Google account. Without signing in, the game still runs as a single-player cave in that browser.
- Your cave and your group are saved to your Google account. On another device, just sign in: the cave and the group come back by themselves.
- The bottom of the page shows when the cave was last saved online, or why saving failed.
- Anyone who has the group code can join that group, so don't post it publicly.
- Visitors unlock as your all-time calcite grows, from a bat after a few hours to a dragon after many weeks. Each kind has its own waiting time between sends (6 hours for a bat, 2 weeks for a dragon). The limit is enforced by the page, not the server.
- Firebase's free Spark plan is far more than two players need.
