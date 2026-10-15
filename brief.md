Version: NAFATH-V2
Track: Software

## Overview

A telecom operator is launching a 5G service for emergency responders. The service must stay reliable during congestion, outages and cyber attacks, and each IT field protects one part of that goal:

- **Networking** gives responders a dedicated 5G slice with its own priority.
- **AI** predicts congestion, so the operator can act early.
- **Software** makes automatic retries safe, without duplicate actions.
- **Mobile** keeps the responder app working offline.
- **Cybersecurity** gives a clear plan to detect, contain and recover from attacks.
- **Data** turns daily usage per region into reports that show where the service is under stress.

Together, these fields protect the service at every layer, from the network to the responder's hand.

## Software

**Short answer:** safe methods only read data; idempotent methods can be repeated with the same intended effect.

- **Safe:** GET, HEAD, OPTIONS and TRACE.
- **Idempotent:** all safe methods, plus PUT and DELETE.
- **Not guaranteed idempotent:** POST and PATCH.

**Why it matters:** when a responder's request times out, the app can retry a GET or PUT automatically. Retrying a POST could create a duplicate incident report, so it needs extra protection.

Resource: RFC 9110 - HTTP Semantics (Section 9.2 Common Method Properties) - https://www.rfc-editor.org/rfc/rfc9110

## AI

**Overfitting:** the model learns its training data too closely, so it scores well on data it has seen but fails on new data. Here, that means false or missed congestion alerts.

**How k-fold cross-validation helps:**

1. Split the data into k folds.
2. Train on k-1 folds and validate on the remaining fold.
3. Repeat k times and average the scores.

**Warning sign:** a high training score with a much lower validation score shows overfitting.

Resource: 3.1. Cross-validation: evaluating estimator performance - scikit-learn documentation - https://scikit-learn.org/stable/modules/cross_validation.html

## Mobile

**While offline:** the app works offline-first. A local database is the single source of truth, so maps, cases and contacts stay readable without a network. Online-only actions are clearly disabled.

**When the connection returns:**

1. New reports are saved locally first, then queued.
2. A background worker (WorkManager on Android) sends the queue and retries with exponential backoff.
3. Conflicts are resolved with "last write wins", using timestamps.

Resource: Build an offline-first app - Android Developers - https://developer.android.com/topic/architecture/data-layer/offline-first

## Networking

## Cybersecurity

## Data

## Misconception in My Track

## Open Question
