# ExfilPoC — iOS Data-Exposure Research

> **Research and defensive analysis only.** This repository documents the security and privacy risks of an iOS proof of concept. It does not publish the operational collection, persistence, or exfiltration implementation.

## Scope

The supplied project describes an iOS app and a local Python receiver. Its components demonstrate how sensitive information can be exposed when an application combines:

- Text-change observation and collection of values already present in form fields
- Clipboard polling
- Screenshots of the app's own foreground window
- Local buffering of collected records
- Periodic and background-triggered upload attempts
- A receiver that stores uploaded payloads and images

The original material explicitly demonstrates that secure-field text may be recorded as plaintext in the JSON payload. Treat that behavior as a critical privacy and security risk, not as a recommended implementation.

## Repository contents

- [Security review](docs/SECURITY_REVIEW.md) — source-grounded findings and impact
- [Defensive detection guidance](docs/DETECTION_GUIDANCE.md) — monitoring ideas for defenders
- [Safe test plan](docs/SAFE_TEST_PLAN.md) — synthetic-data-only validation

## Safety boundary

This repository intentionally excludes code that captures credentials, harvests clipboard contents, takes screenshots for remote collection, persists covertly, or sends collected user data to a command-and-control receiver.

Use synthetic data in an isolated test environment. Do not test against another person's device, accounts, applications, or information without explicit authorization.

## Platform note

The source describes capture of the app's own UIKit window; it does not establish that an ordinary iOS app can silently screenshot other apps. iOS background execution is controlled by the operating system and is not a guaranteed periodic scheduler.

## Source basis

This documentation is based on the supplied ExfilPoC project description and source excerpts. It is a research summary, not a claim that the project has been independently built or executed.
