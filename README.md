# SHUTR LAB — Release Channel

This repository hosts public releases and the [Sparkle](https://sparkle-project.org)
update feed for the **SHUTR LAB** macOS tethering app for Canon EOS cameras.

The app source code is **not** published here.

## Files

- `appcast.xml` — the Sparkle update feed. The app fetches this URL to learn
  about new versions:

      https://raw.githubusercontent.com/conquerdesigngroup/shutr-lab/main/appcast.xml

- `Releases/` (GitHub Releases tab) — the signed, notarized `.dmg` files
  Sparkle downloads when the user accepts an update.

## Cutting a release

After running `dist/build-dmg.sh` from the app source repo:

1. Note the printed Sparkle EdDSA signature (`sparkle:edSignature`) and DMG
   byte length.
2. Create a new GitHub Release tagged `vX.Y.Z` and upload `SHUTR-LAB-X.Y.Z.dmg`
   as the release asset.
3. Append a new `<item>` to `appcast.xml`:

   ```xml
   <item>
     <title>Version X.Y.Z</title>
     <pubDate>RFC-822 date</pubDate>
     <sparkle:version>BUILD_NUMBER</sparkle:version>
     <sparkle:shortVersionString>X.Y.Z</sparkle:shortVersionString>
     <sparkle:minimumSystemVersion>13.0</sparkle:minimumSystemVersion>
     <description><![CDATA[Release notes here.]]></description>
     <enclosure
       url="https://github.com/conquerdesigngroup/shutr-lab/releases/download/vX.Y.Z/SHUTR-LAB-X.Y.Z.dmg"
       sparkle:edSignature="..."
       length="DMG_SIZE_IN_BYTES"
       type="application/octet-stream" />
   </item>
   ```

4. Commit + push `appcast.xml`. Existing app installs check the feed on launch
   (Sparkle default) and prompt the user.
