# How to Connect to the Shared Folder

End-user guide given to each team member. Real addresses and logins were removed from this public copy; each user gets a filled-in version privately.

## Before you start

1. **Install Tailscale** on your device (tailscale.com/download, or the App Store on iPhone).
2. **Sign in** with the email address the invite was sent to, and accept the shared server.
3. **Turn Tailscale on.** It must say Connected whenever you use the shared folder.
4. **Have your connection info ready:**

| Item | Your info |
| --- | --- |
| Mac address | `smb://<server-tailscale-ip>/shared` |
| Windows address | `\\<server-tailscale-ip>\shared` |
| iPhone address | `smb://<server-tailscale-ip>` |
| Username | (provided privately) |
| Password | (sent separately) |

Your username and password are for the file server, not your Tailscale or Apple account.

## Mac

1. Open **Finder**.
2. In the top menu, click **Go** → **Connect to Server** (or press Command + K).
3. Type your **Mac address** from the table above, then click **Connect**.
4. Choose **Registered User**, enter your username and password, and check **Remember this password in my keychain**.
5. Click **Connect**. The shared folder opens in Finder.
6. To find it easily later, drag the folder into the **Favorites** section of the Finder sidebar.

## Windows PC

1. Open **File Explorer**.
2. Right-click **This PC** in the left sidebar and choose **Map network drive**.
3. **Drive:** pick any letter (for example, S:).
4. **Folder:** type your **Windows address** from the table above.
5. Check **Reconnect at sign-in** and **Connect using different credentials**, then click **Finish**.
6. Enter your username and password, check **Remember my credentials**, and click **OK**.
7. The shared folder now appears under **This PC** as its own drive.

## iPhone

The private version of this guide includes a QR code that opens the server address directly.

1. Open the **Tailscale** app and make sure it says **Connected**.
2. Open the **Files** app and tap **Browse**.
3. Tap the **…** (three dots) at the top right, then **Connect to Server**.
4. Type your **iPhone address** from the table above, then tap **Connect**.
5. Choose **Registered User**, enter your username and password, and tap **Next**.
6. Tap the **shared** folder. Next time, find it under **Shared** in the Browse tab.

## If it doesn't connect

| Problem | Fix |
| --- | --- |
| Can't find the server | Open Tailscale and confirm it says Connected. |
| Login rejected | Retype your file server username and password, not your Tailscale or Apple login. |
| Worked before, stopped now | Turn Tailscale off and back on, then reconnect. |
| Still stuck | Send the admin a screenshot of the error message. |
