# SHUTR LAB — Release Channel

This repository hosts public releases and the [Sparkle](https://sparkle-project.org)
update feed for the **SHUTR LAB** macOS tethering app for Canon EOS cameras.

The app source code is **not** published here.

## Files

- `appcast.xml` — Sparkle update feed. The app fetches this URL on launch:

      https://raw.githubusercontent.com/conquerdesigngroup/shutr-lab/main/appcast.xml

- **GitHub Releases tab** — signed, notarized `.dmg` files Sparkle downloads
  when the user accepts an update.

## Cutting a release

From the app source repo (`MM TETHER APP/`):

1. Bump `MARKETING_VERSION` and `CURRENT_PROJECT_VERSION` in `project.yml`.
2. Run `./dist/build-dmg.sh`. It builds, notarizes, and **prints a ready-to-paste
   `<item>` block for `appcast.xml`** at the end (with the Sparkle EdDSA
   signature already embedded).
3. Create a GitHub Release and upload the DMG:

       gh release create vX.Y.Z dist/SHUTR-LAB-X.Y.Z.dmg --title "vX.Y.Z" --notes "Release notes"

4. Paste the printed `<item>` block at the top of `<channel>` in `appcast.xml`,
   replace the placeholder release notes, commit, and push:

       cd ~/Desktop/shutr-lab
       $EDITOR appcast.xml
       git commit -am "vX.Y.Z" && git push

That's it. Existing app installs check the feed and prompt the user on the next
launch (or whenever they hit "Check for Updates" at the bottom of the app).

## Sparkle key recovery

The EdDSA private key used to sign updates lives in the developer's macOS
**login Keychain** (added by Sparkle's `generate_keys` tool). If that keychain
is lost, no future updates can be issued under the current public key —
recovery requires shipping a new app version with a new `SUPublicEDKey` so
clients can transition.

Public key embedded in the app's Info.plist:

    aeDu0SM0Nrh6s05TGxTlvSj/tM9x5LLUetkcBfDD7xM=
