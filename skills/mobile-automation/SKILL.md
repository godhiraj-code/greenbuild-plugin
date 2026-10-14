---
name: mobile-automation
description: Set up, write, and debug mobile UI tests with Appium (UiAutomator2, XCUITest drivers), Maestro YAML flows, and native Espresso/XCUITest, and pick the right tool per team.
---
# Mobile automation
## 1. Choose the tool
- Appium: cross-platform, any language, works with existing Selenium skills. Best for large suites and device clouds.
- Maestro: YAML flows, quick to write, built-in waits/retries. Best for smoke and E2E flows owned by mixed teams.
- Espresso (Android) / XCUITest (iOS): native, fastest and most stable, but the app's own devs have to own them.

## 2. Appium setup (current npm release is Appium 3.x; driver commands are the same as in 2.x)
```bash
node -v   # Appium 3 needs Node ^20.19 || ^22.12 || >=24
npm i -g appium
appium driver install uiautomator2   # Android
appium driver install xcuitest       # iOS (macOS + Xcode)
appium driver doctor uiautomator2    # check env (ANDROID_HOME, JDK)
appium                               # server on http://127.0.0.1:4723 (no /wd/hub since v2)
```
Python client (`pip install Appium-Python-Client`):
```python
from appium import webdriver
from appium.options.android import UiAutomator2Options
from appium.webdriver.common.appiumby import AppiumBy
opts = UiAutomator2Options()
opts.app = "/path/app.apk"; opts.device_name = "emulator-5554"
opts.auto_grant_permissions = True
driver = webdriver.Remote("http://127.0.0.1:4723", options=opts)
driver.find_element(AppiumBy.ACCESSIBILITY_ID, "login").click()
```
Use `XCUITestOptions` for iOS (platformVersion, udid, bundleId or app path).

## 3. Appium good practice
- Locator priority: accessibility id > resource-id / iOS predicate or class chain > XPath (last resort). Ask the devs to add test IDs.
- Use explicit waits (WebDriverWait). Avoid implicit waits plus sleeps.
- Reset state on purpose: `noReset`/`fullReset`, or clear app data per test. Use deep links to skip set-up screens.
- Use W3C Actions or `mobile:` gestures (e.g. `mobile: swipeGesture` on UiAutomator2) instead of the removed TouchAction API.
- Capture screenshots, page source and device logs on failure.

## 4. Maestro
```bash
curl -fsSL "https://get.maestro.mobile.dev" | bash
```
```yaml
appId: com.example.app
---
- launchApp:
    clearState: true
- tapOn: "Sign in"
- inputText: "user@example.com"
- tapOn:
    id: "password"
- inputText: ${PASSWORD}
- assertVisible: "Welcome"
```
Run `maestro test flows/` (pass `-e PASSWORD=...`). Use `maestro studio` to inspect elements and `runFlow` to share sub-flows.

## 5. Native
- Espresso: `./gradlew connectedAndroidTest`. Use IdlingResources for async work.
- XCUITest: `xcodebuild test -scheme App -destination 'platform=iOS Simulator,name=iPhone 16'`. Use accessibilityIdentifier.

## 6. Debug checklist
Stale element? Re-find it after navigation. Keyboard covering the element? Hide the keyboard. Different behavior on a real device? Check permissions dialogs and the OS version. Slow session start? Pre-install the app or use a device cloud.
