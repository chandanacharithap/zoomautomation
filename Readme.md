# Zoom Automation UI Tests (Xcode)

This repo contains a `ZoomDualUITests.swift` file that automates a **Host** and **Client** Zoom flow using **Xcode UI Tests**, looping through WireGuard VPN tunnels.
---

## Setup (from scratch)

### 1) Create a new Xcode project
1. Xcode → **File → New → Project…**
2. Choose **iOS App**
3. Name it anything (example: `wireguardZoomautomationtest`)
4. ✅ Make sure **Include Tests** is checked (this creates *UITests* target)

---

### 2) Add the test file from this repo
1. In Xcode Project Navigator:
   - **File → Add Files to "<YourProjectName>"…**
2. Select **`ZoomDualUITests.swift`** from this repo
3. In the “Choose options for adding these files” popup:
   - ✅ Check **`<YourProjectName>UITests`**
   - ❌ Uncheck the **App** target (`<YourProjectName>`)
   - ❌ Uncheck the **Unit Tests** target (`<YourProjectName>Tests`)
4. Click the file you added → right panel **File Inspector** → **Target Membership**
   - ✅ `<YourProjectName>UITests` must be checked

> If you see `No such module 'XCTest'`, it means the file is attached to the **App target** instead of **UITests**. Fix Target Membership as above.

---

## One-time device setup (first run on a phone)

### 3) Pair / trust the iPhone with Xcode
1. Connect your iPhone to your Mac using a **cable**
2. On iPhone:
   - Trust the computer when prompted
   - **Settings → Privacy & Security → Developer Mode → ON** (restart if asked)
3. In Xcode:
   - **Product → Destination → Manage Run Destinations…**
   - Ensure your iPhone appears and is usable

---

## Project settings (recommended)

### 4) Set Deployment Target for all targets
1. In Xcode, double-click the blue `.xcodeproj` entry (your project name with the Xcode icon)
2. Go to **General**
3. For **each target** (App / Tests / UITests), set:
   - **Deployment Info → iOS = 18.4** (you can type it)

---

## Running

### 5) Build to your phone
1. At the top toolbar, choose the **Destination** dropdown → select your iPhone
2. Click the **Run (▶︎)** button once (this builds + installs the runner on the phone)

### 6) Run UI tests
You have two ways:

**Option A (recommended, explicit):**
- Open **Test Navigator** (⌘6)
- Click the **◇ diamond** next to:
  - `testHost_RunAllTunnels()` on the *Host phone*
  - `testClient_RunAllTunnels()` on the *Client phone*

**Option B (⌘U):**
- Press **⌘U** to run tests on the current destination.

> If you have 2 phones connected and Xcode runs tests on both, it’s because the Test Plan has **Execute in parallel** enabled.  
> To make ⌘U deterministic, open the Test Plan and disable **Execute in parallel** for UITests.

---

## Pre-reqs inside Zoom (to avoid flaky failures)
- Zoom installed and **signed in** on both phones
- Open Zoom once manually to clear first-run popups (camera/mic/notifications)
- Host phone must have a contact named exactly:
  - `chandana charitha peddinti`  
  (or change `contactName` in the test file)
- Keep phones awake (Auto-Lock = Never temporarily helps)

---

## VPN tunnels (optional)
If you use WireGuard VPN loops, the tunnel names in iOS Settings must match the arrays in the test file:
```swift
private let hostTunnels   = ["rtc-australia-east"]
private let clientTunnels = ["rtc-central-us","rtc-japan-east","rtc-germany-west-central"]

## Suggested Repo Layout

```text
your-repo/
├─ wireguardZoomAutomation.xcodeproj
├─ wireguardZoomAutomation.xctestplan
├─ wireguardZoomAutomation/                 # stub app target (untouched)
└─ wireguardZoomAutomationUITests/
   └─ ZoomDualUITests.swift                 # UI test code

That’s it—everything is runnable via **Xcode GUI**: set the **Destination** iPhone at the top, then click the **diamond** next to **Host** or **Client** in the **Test Navigator**. 

Note: Please add sleep(5) as the first line in private func clientFlow(on app: XCUIApplication) function so that the automation will run smoothly.
