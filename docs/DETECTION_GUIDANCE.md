# Defensive Detection Guidance

This document describes defensive review ideas for application owners, mobile security teams, and incident responders. It does not provide instructions for building or concealing a collection agent.

## Review the app's declared behavior

- Inventory background modes and scheduled background tasks.
- Review whether clipboard access is necessary and clearly explained to the user.
- Review screenshot or image-rendering features and whether capture is user initiated.
- Inspect local storage for unexpected logs, queues, or retained images.
- Review network destinations and the purpose of each upload.

## Network-side review

Where authorized telemetry is available, investigate:

- Unexpected periodic uploads from an app that has no clear telemetry requirement
- Multipart requests that combine structured text data and image files
- Upload destinations that do not match the app's documented service providers
- Large or repeated uploads occurring near app backgrounding or foreground transitions
- Receiver endpoints that lack authentication, authorization, retention controls, or audit logs

These are investigative leads, not proof of malicious activity. Validate them against the app's documented functionality and user consent.

## Device and application review

- Check whether app-private storage contains unexpected text logs, JSON payloads, image files, or upload queues.
- Compare observed permissions and behavior with the app's privacy disclosures.
- Confirm that sensitive fields are excluded from diagnostics and analytics.
- Review crash reports and diagnostic exports for accidental inclusion of form contents.
- Confirm that users can understand, control, and stop any data-sharing feature.

## Incident response checklist

1. Preserve relevant logs and timestamps using the organization's approved process.
2. Identify the app version, device scope, observed destinations, and affected time window.
3. Determine what categories of data may have been collected; avoid opening or copying sensitive content unnecessarily.
4. Restrict or disable the affected app or receiver while preserving evidence.
5. Rotate credentials if there is evidence they may have been exposed.
6. Review server-side access logs and determine whether uploaded data was accessed by others.
7. Apply retention and deletion procedures consistent with legal and organizational requirements.
8. Document confirmed findings separately from hypotheses.

## Important caveat

No single API, background task, request shape, or file name proves malicious behavior. Detection should combine technical evidence, expected application behavior, authorization context, and user reports.
