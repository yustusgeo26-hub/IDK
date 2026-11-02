# Safe Test Plan

## Objective

Validate privacy controls and defensive monitoring using a deliberately benign app and synthetic information. Do not reuse credential-capture, clipboard-harvesting, covert screenshot, persistence, or remote collection components from the original PoC.

## Test environment

- Use an iOS simulator or a dedicated test device.
- Use a local test service bound to loopback or an isolated lab network.
- Use fabricated names and strings such as `TEST_USER_001` and `NOT_A_REAL_PASSWORD`.
- Do not sign in to real accounts or paste real personal, financial, or work information.
- Do not install the test app on another person's device.

## Test cases

### A. Sensitive-field exclusion

- Enter a synthetic value into a password field.
- Verify that application diagnostics and analytics do not record the value.
- Verify that logs contain no password text, even when a diagnostic event occurs.

### B. Clipboard minimization

- Keep clipboard access disabled unless the user explicitly invokes a visible paste action.
- If a visible paste feature exists, verify that only the user-selected paste action is handled.
- Verify that clipboard contents are not polled or transmitted in the background.

### C. User-controlled screenshot sharing

- Require the user to tap a visible capture control.
- Show a preview before any share action.
- Verify that canceling the preview results in no upload.
- Verify that sensitive screens are excluded.

### D. Local data retention

- Inspect the app's test storage after a session.
- Confirm that no sensitive text or images are retained.
- Verify that temporary test artifacts are removed according to the documented retention policy.

### E. Network transparency

- Send only a harmless synthetic health-check event to the local test service.
- Confirm that the destination and data categories are documented.
- Confirm that the receiver rejects unauthenticated or malformed requests in any environment where authentication is required.

## Pass criteria

- No password or sensitive field values appear in logs, queues, screenshots, or network payloads.
- No background clipboard polling occurs.
- Screenshot sharing is visible, user initiated, and cancellable.
- Network destinations are documented and expected.
- Temporary test data is deleted according to the stated retention policy.

## Reporting

Record the test date, app build, device or simulator version, test case, expected result, observed result, and evidence location. Use synthetic values only in screenshots and logs.
