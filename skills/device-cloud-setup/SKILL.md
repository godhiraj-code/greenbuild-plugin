---
name: device-cloud-setup
description: Run web and mobile tests on BrowserStack (Automate, App Automate, Local, reporting/observability), Sauce Labs, or LambdaTest, with capabilities, tunnels, parallelism, and CI secrets.
---
# Device cloud setup
Credentials always come from env/CI secrets: BROWSERSTACK_USERNAME / BROWSERSTACK_ACCESS_KEY, SAUCE_USERNAME / SAUCE_ACCESS_KEY, LT_USERNAME / LT_ACCESS_KEY.

## BrowserStack via the SDK (Python example; Java, Node and others are similar)
```bash
pip install browserstack-sdk
browserstack-sdk setup --username "$BROWSERSTACK_USERNAME" --key "$BROWSERSTACK_ACCESS_KEY"
browserstack-sdk pytest -s tests/     # SDK injects caps + parallels from browserstack.yml
```
browserstack.yml:
```yaml
projectName: Web Shop
buildName: ${CI_PIPELINE_ID}
platforms:
  - os: Windows
    osVersion: 11
    browserName: Chrome
    browserVersion: latest
  - deviceName: Samsung Galaxy S23
    osVersion: 13.0
    browserName: chrome
parallelsPerPlatform: 2
browserstackLocal: true     # for localhost/staging behind firewall
debug: false
networkLogs: true
testObservability: true     # test reporting/analytics dashboard
```
## Without the SDK: W3C capabilities
Hub `https://hub.browserstack.com/wd/hub`, with vendor options under `bstack:options` (os, osVersion, projectName, buildName, sessionName, local, debug).

## App Automate (mobile)
```bash
curl -u "$BROWSERSTACK_USERNAME:$BROWSERSTACK_ACCESS_KEY" \
  -X POST "https://api-cloud.browserstack.com/app-automate/upload" -F "file=@app.apk"
# returns {"app_url":"bs://..."} -> set `app: bs://...` in browserstack.yml
```
Then pick real devices (deviceName + platformVersion) and run your Appium suite through the SDK.

## Local testing
Set `browserstackLocal: true` (the SDK starts the tunnel). Use `localIdentifier` when several jobs run tunnels in parallel.

## Mark pass/fail
The SDK marks sessions automatically. Without it, run `driver.execute_script('browserstack_executor: {"action":"setSessionStatus","arguments":{"status":"failed","reason":"..."}}')`.

## Sauce Labs (brief)
Hub `https://ondemand.us-west-1.saucelabs.com/wd/hub` (pick your data center). Options go under `sauce:options` (build, name, tunnelName). Tunnel: Sauce Connect.

## LambdaTest (brief)
Hub `https://hub.lambdatest.com/wd/hub`. Options go under `LT:Options` (build, name, tunnel). Tunnel: LambdaTest Tunnel.

## Checklist
1. Align parallel count with the plan limit.
2. Name each build after the CI run.
3. Use a short device/browser matrix based on analytics, not "everything".
4. Upload video/log links into the report.
5. Fail fast on capability errors.
6. Watch the queue time: it is wasted minutes.
