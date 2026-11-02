# Security Review

## Executive summary

The supplied PoC combines sensitive-data collection, local persistence, and network transfer. In particular, the described text observer records complete text snapshots from text fields and text views. The sample flow includes a secure password field, and the source says its value is serialized into the payload as plaintext. This is a critical confidentiality risk.

## Findings

### 1. Sensitive form values can be recorded in plaintext

**Evidence in supplied source:** `KeyLogger` records the full text of `UITextField` and `UITextView` values. The record includes an `isSecure` flag, but the text value is still included. The run instructions explicitly describe the password as plaintext in `payload.json`.

**Impact:** Credentials and other sensitive form contents may be exposed in local logs, queued payloads, receiver storage, backups, or later handling.

**Defensive recommendation:** Never collect password-field contents. Avoid collecting complete text snapshots. For legitimate diagnostics, use explicit user consent and synthetic values, and record only the minimum non-sensitive event metadata required.

### 2. Clipboard content is included in collection

**Evidence in supplied source:** `ClipboardHarvester` polls the general pasteboard and returns changed string content. The orchestration layer places that value in the outgoing envelope.

**Impact:** Clipboard data can include passwords, authentication codes, personal messages, financial details, or copied work material.

**Defensive recommendation:** Do not poll or transmit clipboard contents in a background collection workflow. If a user-initiated paste feature is required, access only the value the user intentionally pastes into the visible feature and explain the behavior.

### 3. App-window screenshots are queued for upload

**Evidence in supplied source:** `ScreenCapture` renders the app's active key window. The agent can attach the resulting JPEG to a queued payload.

**Impact:** Screenshots can disclose information visible in the app, including personal or confidential content.

**Defensive recommendation:** Make capture user-initiated, visibly indicate when it occurs, preview the image, and require confirmation before sharing. Exclude password and other sensitive screens.

### 4. Collected data is persisted before transmission

**Evidence in supplied source:** The text log and upload spool are disk-backed. The spool stores a JSON payload and optional image before upload.

**Impact:** Sensitive data can remain on the device after a network failure or interrupted session. A local queue increases the number of places where data may be exposed.

**Defensive recommendation:** Do not persist sensitive content. For legitimate telemetry, minimize fields, set short retention limits, protect stored data, and provide a clear deletion path.

### 5. Receiver accepts and stores uploaded payloads

**Evidence in supplied source:** The Python receiver accepts multipart uploads and writes payload and image files into numbered capture directories.

**Impact:** The receiver becomes a repository of sensitive data. The supplied excerpt does not show authentication, authorization, retention controls, or access auditing.

**Defensive recommendation:** A production receiver must authenticate clients, authorize each upload, validate size and content, restrict network exposure, encrypt data in transit and at rest, log access, and enforce retention and deletion policies.

### 6. Background scheduling is not guaranteed

**Evidence in supplied source:** The project uses iOS background task scheduling and describes the refresh cadence as opportunistic.

**Impact:** Background execution should not be treated as a reliable timer or proof of persistent execution.

**Defensive recommendation:** Design ordinary apps around documented, user-visible iOS lifecycle behavior. Do not attempt to evade platform controls.

## Overall risk

The combination of broad text capture, clipboard collection, screenshot capture, disk-backed queuing, and remote upload creates a high-risk data-exposure design. It should not be deployed on personal or production devices.

## Scope and limitations

This review is based on the supplied source excerpts. It is not a full source-code audit, binary analysis, penetration test, or independent runtime verification.
