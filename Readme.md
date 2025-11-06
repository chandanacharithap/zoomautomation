# Zoom Dual UI Automation (Xcode GUI-Only)

Automated Zoom meeting flows for **Host** and **Client** using Xcode UI Tests—no Terminal needed. Runs entirely from the **Test Navigator** using the little **diamonds**.

---

## Prereqs (one-time)

* **Two iPhones** (or one, run separately):

  * Developer Mode enabled (Settings → Privacy & Security → Developer Mode).
  * Trusted on your Mac.
* **Zoom** installed and signed in on both phones (`us.zoom.videomeetings`).
* **Contact** on the host phone named exactly: `chandana charitha peddinti`
  (or change `contactName` in the test file).
* **VPN tunnels** added in iOS Settings → VPN (names must match the code if you use the VPN loops).
* iOS language **English** (labels like “Participants”, “End”).
* Xcode 15+ recommended.

---

## Project Setup (from scratch)

1. **Xcode → File → New → Project… → iOS App**
   Name it anything (e.g., `wireguardZoomAutomation`). ✅ **Include Tests** must be checked.
2. Convert to **Test Plans**:
   Scheme menu → **Edit Scheme…** → **Test** → “**Convert to use Test Plans…**” (accept defaults).
3. In the Project Navigator, under your **UITests target** (e.g., `…UITests`):
   **Right-click → New File… → Swift File** → name it **`ZoomDualUITests.swift`**.
   Paste the test code there.
   In **File Inspector → Target Membership**, ensure ✅ the **UITests** target is checked.
4. Open the **.xctestplan** file →

   * **Configurations → Default Configuration**

     * **Target** = your **UITests** bundle (not the app).
     * **Language/Region** = English (US).
     * **Application** = leave empty (tests launch Zoom/Settings by bundle ID).

---

## How to Run 

1. Plug in/select the iPhone you want as your **Destination**:
   In the top toolbar (next to Run/Stop), click the **device selector** and pick the iPhone.
2. Open **Test Navigator**: **⌘6** (or View → Navigators → Show Test Navigator).
3. Expand: **`<YourUITestsTarget>` → `ZoomDualUITests`**. You’ll see:

   * ◇ `testHost_RunAllTunnels` (or `testHost_RunSingle`, depending on your file)
   * ◇ `testClient_RunAllTunnels` (or `testClient_RunSingle`)
4. **Click the diamond ◇** next to the single test you want to run.

   * To run **Host**: set Destination to the **host iPhone**, then click the **Host** diamond.
   * To run **Client**: set Destination to the **client iPhone**, then click the **Client** diamond.
5. Watch the test in the **Report** or **Debug** area. Rerun as needed by clicking the diamond again.

> Tip: If the toolbar Destination is wrong, the diamond will run on the wrong device—always set the Destination first.

---

## Config You Can Tweak (in `ZoomDualUITests.swift`)

At the top of the file:

```swift
private let contactName       = "chandana charitha peddinti"
private let callSecondsHost   = 20
private let callSecondsClient = 20

// If your version includes VPN and multi-VM loops:
private let hostTunnels   = ["rtc-australia-east"]
private let clientTunnels = ["rtc-central-us","rtc-japan-east","rtc-germany-west-central"]
```

Change these to match your contact/tunnels/durations.

---

## First-Run Checklist (avoids flaky clicks)

* Open Zoom **once** on each phone to clear first-run popups (camera/mic/notifications).
* Ensure **Waiting Room** is configured so “Admit” appears.
* Verify the host phone actually has the **contact** (or update `contactName`).
* Keep phones **awake & unlocked** while testing.

---

## Troubleshooting Quickies

* **“Participants”/“End” not found** → The UI sometimes hides controls. The test taps to reveal; if it still fails, open Zoom manually once and try again.
* **“Join” duplicate issue (client)** → The test picks the best/visible “Join”; ensure the invite card is on screen (no overlapping panels).
* **VPN fails** → First-time iOS alerts can block the toggle; open Settings → VPN once manually to allow permissions.

---

## Suggested Repo Layout

```
your-repo/
├─ wireguardZoomAutomation.xcodeproj
├─ wireguardZoomAutomation.xctestplan
├─ wireguardZoomAutomation/                # (stub app target, untouched)
└─ wireguardZoomAutomationUITests/
   └─ ZoomDualUITests.swift                # <— your test code
```

That’s it—everything is runnable via **Xcode GUI**: set the **Destination** iPhone at the top, then click the **diamond** next to **Host** or **Client** in the **Test Navigator**. Push this repo to GitHub as-is (with the `.gitignore`) and you’re good.
