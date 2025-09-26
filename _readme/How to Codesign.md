**Codesign and Notarize Max Standalone for distribution outside of the Mac App Store.**

**Setup keychain access** (https://developer.apple.com/account/resources/certificates/add)

1. Download the Apple Worldwide Developer Relations Certification Authority **Intermediate Certificates AppleWWDRCAG3.cer**.

2. Select this Apple Worldwide Developer Relations Certification Authority cert in keychain, and go to Keychain Access>Certificate Assistant>**Request a certificate from the certificate authority**

3. Complete the steps for vj@vjmanzo.com

4. Use the CSR to complete the **Developer ID Application** certificate generation; then download and install that certificate

5. From a **terminal** run the command: **security find-identity -v -p codesigning.** If the new **Developer ID Application** identity is not found in the list, troubleshoot your certificates and keychain.

**Build the application**

6. **Build** the Discourse standalone.

7. **Copy** the entitlements file (see below) to **Contents/Discourse.entitlements**.

**Codesign**

8. Recursively clear the extended attributes of Discourse.app by running **xattr -cr /Users/VJ/Desktop/Discourse.app**

9. Codesign all files in the app by running **ruby /Volumes/Media/Git\ Repos/_GitHub/discourse/_readme/sign.rb /Users/VJ/Desktop/Discourse.app** 

**Notarize**

10. [SKIP AFTER SETUP] Create a new **application specific password** in the **Apple ID portal** for your developer account. ([https://support.apple.com/en-us/102654](https://support.apple.com/en-us/102654))

11. [SKIP AFTER SETUP—just zip Discourse.app] Create a new notary profile by running: **xcrun notarytool store-credentials --apple-id "vj@vjmanzo.com" --team-id "YOURAPPLETEAMID"** Remember the name you assign your profile (name it the name of the app!).

12. Zip the app and begin the notary process: **xcrun notarytool submit /Users/VJ/Desktop/Discourse.zip --keychain-profile "Discourse" --apple-id "vj@vjmanzo.com" --team-id YOURAPPLETEAMID --password "YOUR-APP-SPECIFIC-PASSWORD" --wait** This usually takes a few minutes.

13. Check submission for errors **xcrun notarytool log "d2fd9741-4e0e-4e2f-a9e8-6f5824954078" --keychain-profile "Discourse" developer\_log.json**

14. Correct the errors in developer\_log.json and repeat the steps.



**Resources:**

- https://developer.apple.com/documentation/security/notarizing\_macos\_software\_before\_distribution/customizing\_the\_notarization\_workflow
- https://scriptingosx.com/2021/07/notarize-a-command-line-tool-with-notarytool/
- https://cycling74.com/forums/issue-with-code-signing-mac-standalones-with-hardened-runtime#reply-5ec5bf25545dcb52fb3c166a
- https://cycling74.com/forums/mac-standalone-codesigning-2021-update
- [https://developer.apple.com/account/resources/certificates/add](https://developer.apple.com/account/resources/certificates/add)
- [https://support.apple.com/en-us/102654](https://support.apple.com/en-us/102654)
