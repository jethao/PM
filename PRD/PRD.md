# AirHealth PRD

## Revision History

| Version | Date | Author | Summary of Changes | Source of Change | Affected Sections |
| --- | --- | --- | --- | --- | --- |
| v0.1 | 2026-03-22 | PM Agent | Initial PRD draft created from the AirHealth product definition. Added problem framing, scope, requirements, system states, edge cases, metrics, risks, and phased delivery. | Feature definition in `PM/Designs/feature.md` | Entire document |
| v0.2 | 2026-03-22 | PM Agent | Revised PRD in response to Reviewer Agent feedback. Added explicit entitlement states and permissions, concrete result models for both modes, narrowed Phase 1 sharing scope, clarified disconnect authority and pending sync behavior, and tightened launch metrics with numeric targets and telemetry separation. | Reviewer Agent feedback in `PM/PRD/reviews.md` | Sections 5, 6, 7, 8, 12, 14 |
| v0.3 | 2026-03-22 | PM Agent | Revised PRD in response to Reviewer Agent feedback from review v0.2. Clarified Fat Burning semantics, defined the Phase 1 Apple Health and Health Connect export payload, and promoted cached entitlement freshness to an explicit UX rule. | Reviewer Agent feedback in `PM/PRD/reviews.md` v0.2 | Sections 5, 6, 7, 8, 12, 13 |
| v0.4 | 2026-03-23 | PM Agent | Revised PRD to reflect updated feature constraints around handheld industrial design, simplified mechanical architecture, and airflow conditioning before the sensors. Added explicit hardware, UX, technical, risk, assumption, and delivery requirements so the device form factor and sample path are implementation-ready. | Updated feature definition in `PM/Designs/feature.md` | Sections 1, 4, 5, 7, 10, 11, 13, 14 |
| v0.5 | 2026-03-25 | PM Agent | Revised PRD to integrate the updated home-screen action model, one-action-at-a-time interaction rule, feature-level actions for set goals/view history/measure/get suggestions/consult professionals, and low-power behavior when sensor activity becomes effectively idle. Clarified the corresponding UX, firmware, and operational implications. | Updated feature definition in `PM/Designs/feature.md` | Sections 4, 5, 6, 7, 8, 10, 11, 13, 14 |
| v0.6 | 2026-03-25 | PM Agent | Revised PRD in response to Reviewer Agent feedback. Defined the Phase 1 `consult professionals` action as a curated support directory and contact handoff, and added deterministic hysteresis and debounce rules for low-power entry and exit around the 1% threshold. | Reviewer Agent feedback in `PM/PRD/reviews.md` v0.9 | Sections 5, 6, 7, 8, 10, 11, 13, 14 |
| v0.7 | 2026-03-27 | PM Agent | Revised PRD to incorporate the 3-color LED interaction model, manufacturing-only Factory mode, and internal HW-ID handling. Clarified consumer versus factory scope, one-time factory provisioning behavior, BLE error-log reporting, and the software, firmware, hardware, backend, and support implications of routing outputs by hardware profile and detected VOC type. | Updated feature definition in `PM/Designs/feature.md` | Sections 1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 13, 14 |

## 1. Overview

**Feature name:** AirHealth connected breath analysis platform

**Summary:** AirHealth is a connected consumer electronics system that measures breath-print biomarkers from a dedicated device, transfers results to a mobile app, tracks trends over time, and provides guidance tied to two initial health use cases: Oral & Dental Health and Fat Burning.

**Product context:** The product consists of a handheld hardware breath-analysis device with a 3-color LED, a mobile app, BLE connectivity between device and phone, cloud storage for history and goals, and optional sharing to third-party health ecosystems. The physical device must be shaped for comfortable handheld use, with a simplified internal mechanical architecture and a controlled airflow path that conditions the sample before it reaches the sensors. The product also includes a manufacturing-only Factory mode and an internal HW-ID model so the team can validate hardware before shipment and organize outputs by supported hardware profile and detected VOC type.

**Why this matters:** Users need a repeatable, easy-to-follow way to capture breath measurements, understand trends, and act on guidance without needing to interpret raw sensor data. The system must make a technically complex measurement feel simple, trustworthy, and recoverable when connectivity or setup is interrupted, while also giving manufacturing and support a reliable way to verify hardware health and identify the device variant behind a given result or log.

## 2. Problem Statement

People do not have a convenient way to measure breath-based health signals at home, see progress over time, and translate those signals into understandable guidance. Existing wellness tools often track generalized habits or single-purpose metrics, but they do not provide a dedicated breath-print workflow with persistent history, goal tracking, and product-specific recommendations.

AirHealth solves this by pairing a dedicated device with a mobile app that leads the user through a structured measurement session, stores the result, and shows progress against a chosen health goal.

Why now: Consumers are increasingly comfortable with connected health devices, but they still need lower-friction experiences that combine physical sensing, app guidance, and longitudinal feedback in one product.

## 3. Goals and Non-Goals

### Goals

- Enable users to pair a breath-analysis device with a mobile phone and complete guided measurement sessions.
- Support two initial modes: Oral & Dental Health and Fat Burning.
- Store measurement history, goals, and progress in the cloud so users can review trends over time.
- Provide understandable feedback and suggestions after a completed measurement session.
- Support data sharing to third-party health apps where technically and commercially enabled.
- Support a manufacturing-only Factory mode that runs a one-time hardware functionality check, reports BLE error logs, and confirms pass/fail status before consumer shipment.
- Use HW-ID as internal metadata to route outputs, logs, and support diagnostics by supported hardware profile and detected VOC type.
- Ensure the system clearly handles cancelation, interruption, offline states, and incomplete sessions.

### Non-Goals

- Diagnosing disease or replacing professional medical, dental, or clinical advice.
- Supporting additional measurement categories beyond the two initial modes in this release.
- Allowing simultaneous measurements or multi-user concurrent sessions on one device.
- Creating a standalone device experience without the mobile app.
- Building a full e-commerce platform inside the product. Product purchase suggestions may link out or deep-link, but checkout is out of scope.
- Exposing Factory mode or HW-ID to consumer-facing UI, settings, or result screens.

## 4. Target Users and Use Cases

### Primary users

- Health-conscious consumers who want a simple, repeatable breath-based routine.
- Users who want to monitor oral and dental-related indicators over time.
- Users who want to understand fat-burning related trends during repeated breath sessions.

### Internal users

- Factory operators who need a one-time verification flow before shipment.
- QA technicians who need clear pass/fail hardware confirmation and error logs.
- Manufacturing and support teams who need HW-ID-based routing to group outputs by device variant or detected VOC type.

### Relevant contexts

- At home, typically after brushing teeth for oral mode.
- During a structured session when the user can follow app instructions without interruption.
- In environments where BLE connectivity and phone access are available.
- While holding a compact handheld device that must remain stable enough for guided mouth placement and repeated breathing steps.
- During manufacturing and open-box verification, where authorized personnel need a one-time hardware check with immediate LED and BLE feedback.
- In a home-screen workflow where the user chooses one feature card at a time and then selects an action such as `set goals`, `view history`, `measure`, `get suggestion`, or `consult professionals`.

### Key use cases

- First-time setup, pairing, and feature selection.
- Setting a goal with or without AI-assisted suggestion.
- Performing a single oral/dental measurement session.
- Performing a multi-step fat-burning session.
- Reviewing trend history and progress.
- Recovering from interruption, cancelation, or device/phone disconnect.
- Running manufacturing verification and triaging hardware logs by HW-ID before the device reaches a consumer.

## 5. Feature Definition

AirHealth measures breath-print data using a dedicated device and transfers the data to a phone app. The user can only measure one thing at a time. If another measurement is needed, the current session must be finished or canceled first. The device uses the app and a 3-color LED together for state feedback, but the consumer app remains the primary control surface.

On the home screen, the app lists all available features as separate cards or tiles. Each feature card exposes the same core actions so the user can enter the product from the task they want to do, rather than navigating through a generic settings flow.

In Phase 1, `consult professionals` is a curated support action, not an in-app booking system. It opens a feature-relevant directory of external educational and professional contact resources, such as websites, phone numbers, and support hours, with the user choosing whether to continue to any external destination.

Factory mode is launch scope for manufacturing and QA, not a consumer-facing feature. It is entered through a long-press hardware gesture, runs a one-time device verification flow, and is used to confirm open-box readiness before the product reaches a user.

### Initial feature modes

**Feature 1: Oral & Dental Health**
- User value: maintain oral and dental health.
- Sensors: hydrogen sulfide and methyl mercaptan sensors.
- Session behavior: the user performs a deep breath, places the device in the mouth, and closes the mouth while the app shows measurement progress.
- Output behavior: the app shows an `Oral Health Score` on a 0-100 scale where 50 represents the baseline average of the first 5 valid oral sessions for that mode. Scores above 50 indicate improvement relative to baseline; scores below 50 indicate regression. The app shows progress as the signed delta from baseline and retains a comparable trend history over time.

**Feature 2: Fat Burning**
- User value: understand fat-burning related trends.
- Sensors: acetone and CO2 sensors.
- Session behavior: the user breathes and holds for 10 seconds, then blows into the device, and repeats until the session is complete.
- Output behavior: the app shows a session-relative `Fat Burn Delta` for each completed reading, expressed as signed percentage points relative to the first valid reading in that session. The first reading sets the session baseline at 0%, and repeated measurements within the session are shown as changes from that baseline. Cross-session comparison uses only the final session delta and the best delta reached in each completed session.

**Feature 3: Factory Mode**
- User value: provide a good open-box experience and reliable manufacturing verification.
- Who uses it: authorized factory operators, QA technicians, and service/manufacturing tooling. It is not exposed in consumer UI.
- Entry and exit: press and hold the device button for 10 seconds to enter Factory mode, and press and hold the device button for 10 seconds to exit. The mode is one-time use and is blocked after factory provisioning is complete.
- Behavior: firmware automatically runs the hardware functionality check, turns the 3-color LED orange while the check is in progress, reports error logs over BLE, turns the LED green if no error is found, and turns the LED red if an error is found.
- Scope: the Factory mode flow must be available at launch for manufacturing verification, but it must not be available to consumers after shipment or in normal app workflows.

**Not user-facing feature: HW-ID**
- User value: none directly; this is an internal routing and support identifier.
- Product role: automatically detect the supported hardware profile and organize outputs based on HW-ID and detected VOC type so firmware, backend, analytics, manufacturing, and support can group records correctly.
- Visibility: HW-ID is never shown in consumer UI, and the mobile app must not require it for consumer measurement flows.
- Flow: none from the user perspective; the value is captured or inferred by firmware and used downstream for routing and diagnostics.

### Scope constraints inherited from the feature definition

- A session may only track one mode at a time.
- A new mode cannot start until the current session is finished or canceled.
- The user can only take one action at a time across the full measurement experience, including setup, measurement, results, and support actions.
- The app should guide the user with animations/instructions during measurement.
- A 60-day trial is part of the product experience, followed by a $5.99 monthly subscription unless changed by business policy.
- The device must be designed as a handheld product, and industrial design plus electrical component selection must fit the compact physical structure.
- The internal architecture should minimize moving or serviceable mechanical parts, with passive structures preferred unless a later engineering review proves a part is required for hygiene, safety, or airflow control.
- Airflow must be stabilized before it reaches the sensors, and the design must prevent direct user breath from impinging on the sensors.
- The device interaction model includes the mobile app, the physical button, and a 3-color LED; the app remains the primary control surface for consumer flows.
- The 3-color LED must provide immediate state feedback, including orange / green / red signaling in Factory mode, without exposing manufacturing-only details in consumer UX.
- The device should enter low-power mode when sensor readings are effectively stable, defined as a change of less than 1% from the last second's average for at least 3 consecutive seconds, so the system conserves power during idle periods without interrupting a user-initiated action.
- The device should exit low-power mode only after either a user/app action is received or sensor activity exceeds 2% change from the last second's average for 2 consecutive seconds, which creates a hysteresis band that prevents oscillation near the 1% threshold.
- Factory mode is manufacturing-only launch scope, one-time use, and must be closed to normal consumer workflows after provisioning.
- HW-ID is internal metadata that may be used by firmware, backend, analytics, manufacturing, and support systems to route outputs, but it must not alter the consumer action model or surface as a user-facing identifier.

## 6. End-to-End Experience

### Discovery

- The user learns about the device through packaging, store listing, or app onboarding.
- The product communicates that it is a guided breath analysis device with two initial modes and a trial period.

### Manufacturing and factory verification

- During manufacturing, authorized personnel place the device into Factory mode with the 10-second button hold.
- Firmware automatically runs the hardware functionality check once Factory mode starts.
- The 3-color LED shows orange while the check is running, green when the device passes, and red when the device fails.
- Error logs are reported over BLE so factory tooling can capture them without opening the device.
- Factory mode is one-time use and is not exposed in consumer onboarding, settings, or support flows.
- HW-ID is captured or inferred during provisioning so downstream firmware, backend, analytics, manufacturing, and support systems can organize outputs by supported hardware profile and detected VOC type.

### Setup

- User installs the app on iOS 26+ or Android 16+.
- User creates or signs into an account if required for cloud sync and trial activation.
- User powers on the handheld device and pairs it to the phone via BLE.
- App confirms successful connection and device readiness.
- User selects one initial mode and sets a goal, optionally using AI-assisted suggestions.
- The app presents the selected feature's action row or action sheet with `set goals`, `view history`, `measure`, `get suggestion`, and `consult professionals` so the user can continue from the feature they care about most.
- The setup flow must not expose Factory mode or HW-ID to the consumer.

### Onboarding

- App explains the measurement steps for the selected mode.
- App explains timing, expected posture, and when to stop or retry.
- App makes it clear that only one active measurement session can exist at a time.
- App makes it clear that only one action can be active at a time, and that other actions remain temporarily unavailable while the current action is in progress.

### Measurement

- App shows the device state and the next required user action.
- Device remains the source of sensing and session control.
- The physical measurement path must guide breath through a stable conditioning path before the sensors, so the app can assume a repeatable sample rather than direct breath impact.
- App reflects live measurement progress, session status, and completion state.
- The 3-color LED may provide redundant device-state feedback during measurement, but the app remains authoritative for consumer instructions.
- While a session is active, the app blocks other feature actions, including goal edits, history mutations, suggestions that require a new measurement, and professional-consultation handoff flows that would conflict with the active session.

### Results and progress

- On completion, app shows the measurement result, comparison to previous history, and goal progress.
- App presents suggestions tied to the active mode.
- If a suggestion includes a product recommendation, the app labels it clearly as a recommendation rather than a medical necessity.
- The results screen must return the user to the same feature card and action model they started from, so they can immediately choose to review history, adjust goals, request a suggestion, or consult a professional without losing context.

### Consult professionals

- `consult professionals` is available for both features and is exposed from the feature card, result screen, and history detail screens.
- In Phase 1, the flow is an external educational and support directory with curated links and contact information for professionals or services relevant to the selected feature.
- The flow does not create an appointment, transmit measurement data, or initiate a medical referral in Phase 1.
- The only context passed into the directory is the selected feature and the user platform locale so the app can filter and localize the list; no account identifiers, breath data, or result values are sent.
- The action is disabled during active measurement and while any other action is in progress because the product enforces one-action-at-a-time behavior.
- The action remains available in trial active, paid active, temporary access, and read-only states because it is informational and does not depend on entitlement to view.
- If the user taps an external resource, the app must clearly indicate that they are leaving AirHealth and that any follow-up is handled by the external provider or service.
- Direct appointment booking, referral routing, and data-sharing handoff to a professional are excluded from Phase 1 and remain later-phase scope.

### Measurement result model

#### Oral & Dental Health result model

- The user-facing score is `Oral Health Score`, a 0-100 value where higher is better.
- The first 5 valid completed oral sessions after the user selects the mode establish the baseline.
- While the baseline is still forming, the app labels the state `Baseline building` and shows `1/5` through `5/5` completed baseline sessions.
- After the 5th valid session, the baseline is locked as the arithmetic mean of those 5 completed scores.
- Every subsequent oral result is displayed relative to the locked baseline:
  - `50` means the user is at baseline.
  - `51-100` means better than baseline.
  - `0-49` means worse than baseline.
- Progress is shown as the signed difference from baseline, in points and percentage relative to baseline, for example `+6 points` or `+12% vs baseline`.
- Comparable history includes all completed oral sessions on the same 0-100 scale, with baseline marked as a reference line and the most recent 7, 30, and 90 days available as trend views.

#### Fat Burning result model

- The user-facing session model is `Fat Burn Delta`, a signed percentage-point value relative to the first valid reading in the session.
- The first valid reading in each session is the session baseline and is displayed as `0%`.
- Repeated readings within the same session are displayed as `+/- N% vs session start`, rounded to the nearest whole percent.
- The session summary is the final valid delta captured before the user taps Finish or the device reports completion.
- The app also shows the best in-session delta and the total number of valid readings captured in that session.
- Cross-session comparison is allowed only on the session summary and best-delta values for completed sessions; the app must not compare intermediate intra-session points across different sessions.
- Positive Fat Burn Delta is desirable. It means the breath-print signal is moving in the fat-burning direction relative to the session baseline. Negative Fat Burn Delta means the signal is below the session baseline and does not count as goal progress.
- `Best delta` is the highest valid Fat Burn Delta recorded after the session baseline within the same session. If multiple readings tie for the highest value, the earliest reading is the best delta for display ordering. If all readings are negative, the least negative reading is the best delta.
- The user sets a positive `Fat Burn Target Delta` in percentage points, either manually or via AI suggestion.
- Goal progress during the session is based on the best delta so far, not the most recent reading. The app shows `current delta`, `best delta so far`, and `goal progress toward target` side by side so the user can see both live movement and the locked-in best session result.
- Goal progress percentage is calculated as `best delta so far / Fat Burn Target Delta`, clamped between 0% and 100%.
- The progress label is `Below target` when the best delta so far is below the target, `At target` when it equals the target, and `Above target` when it exceeds the target.
- The final session summary shows `final delta`, `best delta`, `goal target`, and `goal achieved` status. Goal achieved is true when `best delta` is greater than or equal to the session target delta.
- If a user has no target delta set yet, the app still shows the best delta and labels goal progress as `Target not set` rather than inventing a percentage.

### Entitlement model and permissions

The cloud entitlement service is the source of truth for subscription state. The app may cache the last verified entitlement, but any cached state must be treated as temporary when the backend is unavailable.

| State | Start new sessions | View history | Sync pending results | Change goals | Access recommendations |
| --- | --- | --- | --- | --- | --- |
| Trial active | Yes | Yes | Yes | Yes | Yes |
| Paid active | Yes | Yes | Yes | Yes | Yes |
| Expired/read-only | No | Yes | Yes, but only for sessions that were completed while the user had active entitlement or trial access | No | Yes, but only for previously generated or historical recommendations |
| Entitlement-check-pending/offline | No, unless the app has a cached active entitlement that was verified within the last 24 hours | Yes | Yes for already finalized local results; upload remains queued until the backend becomes reachable and verifies entitlement status | No | Yes for cached recommendations and historical guidance |

- If entitlement is expired, the user may still see and upload pending results from sessions that started while entitlement was active, but the app must not start any new sessions.
- If entitlement cannot be verified because the backend is offline, the app must preserve local results and continue to queue sync attempts without discarding data.
- If a cached active entitlement is older than 24 hours and the backend cannot be reached, the app must fall back to read-only behavior.
- The 24-hour cache window is a user-facing freshness rule, not a hidden implementation detail. When verification is unavailable, the app must clearly label the state as `Temporary access` if the cached entitlement is still fresh and `Read-only mode` once the cache is stale or inactive.
- In `Temporary access`, the user can continue to view history and queued results, but the app blocks new session starts and shows that entitlement will need to be rechecked before the next measurement.
- In `Read-only mode`, the app disables start session controls, goal editing, and recommendation actions that depend on a valid entitlement, while keeping synced history visible.

### Ongoing lifecycle

- Results and history are stored in the cloud.
- The user can revisit previous sessions and progress.
- The user can change mode only after ending the current session.
- Subscription state gates continued access after the 60-day trial according to the entitlement model below.

### Failure and recovery

- If pairing fails, the app explains how to retry.
- If the device disconnects mid-session, the session is marked incomplete and no result is stored.
- If the user stops before completion, the session is canceled and no result is stored.
- If connectivity is unavailable, local status should be preserved until synchronization resumes.
- If the app cannot verify entitlement, it preserves queued results and keeps previously synced history accessible, but it blocks new session starts until entitlement is confirmed or a cached active entitlement is still valid.

## 7. Functional Requirements

### Software Requirements

1. The mobile app must support iOS 26+ and Android 16+.
2. The app must support BLE pairing, connection status, and reconnect flows.
3. The app must support account creation/sign-in if cloud sync, trial activation, or cross-device history requires identity.
4. The app must present separate guided flows for Oral & Dental Health and Fat Burning.
5. The app must not allow the user to start a second measurement while one session is active.
6. The app must show live session state, including ready, measuring, paused, canceled, failed, and complete.
7. The app must store goals, history, and progress in the cloud once the user is authenticated and connectivity is available.
8. The app must expose a subscription state with 60-day trial status and post-trial paid status.
9. In Phase 1, the app must support outbound sharing of completed session summaries to Apple Health on iOS and Health Connect on Android, subject to platform permissions. Direct Oura, Fitbit, and similar partner integrations are deferred to later phases.
10. The app must show product suggestions after results, with clear labeling and no hidden purchase flow.
11. The app must support analytics/telemetry for pairing success, session completion, cancelation, device disconnect, and subscription conversion.
12. The app must share only completed session summaries in Phase 1 and must not export raw sensor streams, raw breath traces, subscription state, account identifiers, or purchase recommendations.
13. The app must use platform-specific export mappings that preserve the same user-facing meaning on both Apple Health and Health Connect while omitting unsupported fields rather than approximating them.
14. The app must present each feature as a task hub with feature-level actions for `set goals`, `view history`, `measure`, `get suggestion`, and `consult professionals`, while disabling or deferring any action that conflicts with an active session.
15. The app must ensure the user can only perform one action at a time across setup, measurement, history review, suggestion generation, and professional-consultation entry points.
16. The app must show a clear low-power state in response to sensor inactivity and must avoid interrupting a user-initiated session transition when entering or exiting that state.
17. The app must surface the `consult professionals` directory in Phase 1 as an informational support flow for both feature modes, without transmitting account, breath, or result data to the external destination.
18. The consumer app must not expose Factory mode entry, HW-ID values, or manufacturing-only logs in any consumer-facing screen, setting, or export flow.
19. The software stack must persist Factory mode pass/fail results, BLE error logs, and HW-ID metadata for manufacturing, backend, analytics, and support workflows.
20. The app and backend must treat HW-ID as an internal routing key so records can be grouped by supported hardware profile and detected VOC type without changing the consumer result model.

### Phase 1 sharing/export contract

The export contract is one completed session summary per session. Canceled, failed, or incomplete sessions are not exported.

| Field | Exported | Notes |
| --- | --- | --- |
| mode | Yes | Oral & Dental Health or Fat Burning |
| session_started_at | Yes | Exported as local session metadata |
| session_completed_at | Yes | Exported as local session metadata |
| session_duration_seconds | Yes | Derived from start and completion timestamps |
| completion_status | Yes | Must be `completed` for Phase 1 exports |
| primary_result_value | Yes | `Oral Health Score` for oral mode, `Fat Burn Delta` for fat mode |
| primary_result_unit | Yes | `score` for oral mode, `%` for fat mode |
| baseline_reference_value | Oral only | The locked baseline average used for comparison |
| best_delta_value | Fat only | The highest valid delta achieved in the session |
| measurement_count | Yes | Number of valid readings contributing to the completed session |
| goal_target_value | Yes, if set | Positive target delta or score target |
| source_device_model | Yes | Non-unique device model identifier |
| source_app_version | Yes | App version used to create the export |
| export_timestamp | Yes | Time the export attempt was made |
| raw_sensor_streams | No | Intentionally omitted |
| raw_breath_samples | No | Intentionally omitted |
| intermediate_readings | No | Intentionally omitted |
| subscription_state | No | Intentionally omitted |
| account_email | No | Intentionally omitted |
| account_name | No | Intentionally omitted |
| payment_information | No | Intentionally omitted |
| purchase_recommendations | No | Intentionally omitted |
| AI rationale or prompt text | No | Intentionally omitted |
| device serial number | No | Intentionally omitted |

### Platform mapping differences

- Apple Health exports the same logical summary but only stores `primary_result_value`, `primary_result_unit`, `session_completed_at`, `session_duration_seconds`, `mode`, `measurement_count`, and `goal_target_value` where supported; `baseline_reference_value` and `best_delta_value` are kept as metadata only if the destination supports metadata fields.
- Health Connect exports the same logical summary and may store `baseline_reference_value`, `best_delta_value`, `measurement_count`, and `goal_target_value` as structured fields when supported by the destination schema.
- If a destination does not support a field, the app must omit that field rather than translating it into a different metric.

### Hardware Requirements

1. The device must support BLE communication with the mobile app.
2. The device must support the sensor set needed for the two initial modes:
   - hydrogen sulfide and methyl mercaptan for Oral & Dental Health
   - acetone and CO2 for Fat Burning
3. The device must include a power control mechanism, at minimum an on/off button or equivalent state trigger.
4. The device must expose a ready state and an in-session state that can be represented to the app.
5. The device must support one active measurement session at a time.
6. The device must be able to report session interruptions, sensor errors, and completion status to the app.
7. The device must support consistent measurement timing and sensor warm-up/ready behavior if needed for accuracy.
8. The device must be manufacturable at the target cost profile of $199 retail, so the bill of materials and enclosure design must remain aligned with that target.
9. The device must be implementable as a handheld product with a slim, ergonomic enclosure that supports stable user grip during oral and breath sessions.
10. The internal design must minimize moving or serviceable mechanical parts, with passive structures preferred where they can meet reliability, hygiene, and cost targets.
11. The sample path must condition and stabilize airflow before it reaches the sensors, and the sensors must not be placed in a position where direct user breath impinges on them without conditioning.
12. The enclosure and component selection must be compatible with the industrial design constraint, meaning electrical, optical, and sensing components may not assume a bulky or multi-piece mechanical assembly.
13. The device firmware must support a low-power state when sensor readings are effectively idle, defined as less than 1% change from the last second's average, and must resume measurement responsiveness when user action or sensor activity resumes.
14. The device and app must preserve one-action-at-a-time behavior by rejecting concurrent session commands, queued feature actions, or conflicting mode changes until the current action is resolved.
15. The device firmware must not enter low-power mode while a user-initiated measurement transition is in flight, and it must use hysteresis so low-power entry requires 3 consecutive seconds below the idle threshold while exit requires either user/app action or 2 consecutive seconds above the exit threshold.
16. The device must include a 3-color LED that supports orange, green, and red states for Factory mode and other state feedback cues.
17. The firmware must support a manufacturing-only Factory mode that is entered and exited with a 10-second button hold, runs exactly one hardware functionality check per device, and blocks re-entry after provisioning is complete.
18. The device must report Factory mode error logs over BLE and must attach HW-ID metadata to factory and session outputs so downstream systems can route by hardware profile and detected VOC type.

## 8. System Behavior and States

### Major states

- Powered off
- Powered on, not paired
- Paired, disconnected
- Paired, connected, ready
- Guided setup
- Feature hub
- Active oral session
- Active fat-burning session
- Factory mode
- Factory check running
- Factory pass
- Factory fail
- Factory locked
- Measuring
- Paused or interrupted
- Low power
- Complete
- Canceled
- Failed
- Trial active
- Paid active
- Expired/read-only
- Entitlement-check-pending/offline

### State transitions

- Powered off to powered on: user presses device power control.
- Not paired to paired: user completes BLE pairing in app.
- Connected to ready: device and app handshake succeeds.
- Ready to feature hub: user lands on the home screen where feature cards and action rows are visible.
- Feature hub to guided setup: user selects a feature and then chooses a compatible action such as set goals, view history, measure, get suggestion, or consult professionals.
- Ready to active session: user selects mode and starts guided measurement.
- Powered off to Factory mode: authorized factory tooling performs the 10-second long press before consumer provisioning.
- Factory mode to Factory check running: firmware begins the one-time hardware functionality check.
- Factory check running to Factory pass: the device completes the check, reports no errors, and turns the LED green.
- Factory check running to Factory fail: the device completes the check, reports one or more errors, and turns the LED red.
- Factory pass or Factory fail to Factory locked: provisioning is complete and the one-time Factory mode flow is no longer available.
- Factory locked to powered off: user performs the 10-second exit hold after the result is reported, and the device shuts down while retaining the one-time-use lock for future boots.
- Active session to measuring: device confirms sensing has begun.
- Measuring to complete: user completes the required steps and device validates the sample.
- Measuring to canceled: user stops the session, closes the app, or aborts intentionally before completion.
- Measuring to failed: sensor error, BLE disconnect, or invalid sample.
- Ready, measuring, or paused to low power: sensor activity becomes effectively idle and the device can safely reduce power without losing session context.
- Low power to ready or measuring: user input, app command, or sensor activity above the exit threshold for 2 consecutive seconds resumes normal operation.
- Complete to history stored: app syncs the completed result to cloud when available.
- Trial active to expired/read-only: user reaches the end of the trial without a paid entitlement.
- Paid active to expired/read-only: paid entitlement lapses or is revoked.
- Any entitlement state to entitlement-check-pending/offline: cloud verification is temporarily unavailable.

### Expected responses

- The app must never show a completed measurement without a corresponding device confirmation.
- The app must not accept a mode switch during an active session.
- The system must preserve the last known state after temporary connectivity loss and resume only if recovery is valid.
- The device is authoritative for live sensing, sample completion, and final session result generation.
- The app is authoritative for user-facing state, local queuing, and entitlement-gated actions.
- The cloud is authoritative for entitlement status and the final acceptance of synced results.
- Factory mode is authoritative on the device and must not depend on app visibility or consumer sign-in to complete the hardware check.
- LED state must align with the device-reported Factory pass/fail outcome, with orange for check-in-progress, green for pass, and red for fail.
- If the device completes a session during a disconnect, the device result overrides any tentative app failure state once the session ID is reconciled.
- Pending sync data must remain queued if entitlement is expired or temporarily unverifiable, and the queue may upload automatically once the backend confirms an eligible entitlement for that completed session.

## 9. Edge Cases and Failure Scenarios

- Pairing fails due to BLE permission denial.
- Pairing fails due to no compatible device being found.
- User begins a session, then closes the app.
- Device disconnects while measuring.
- Phone loses network connectivity before cloud sync completes.
- Device battery is too low to complete a session.
- Sensor warm-up or calibration fails.
- User tries to start a second mode while a first mode is active.
- User cancels a session midway through a measurement.
- User changes subscription state while offline.
- Account sign-in mismatch occurs on a previously paired device.
- Third-party health app integration is unavailable or permission is denied.
- A consumer-owned device receives a long press that would otherwise enter Factory mode after provisioning is complete.
- Factory mode reports a hardware failure and the BLE log transfer is interrupted.
- The backend cannot associate a completed session or factory log with a supported HW-ID.
- The 3-color LED indicates a fail state while the app is disconnected or offline.

For every failure case, the app must show:
- what happened
- whether any data was saved
- what the user can do next

## 10. UX and Design Implications

- The setup flow must make pairing and mode selection feel like one guided sequence, not disconnected settings tasks.
- The measurement screen must prioritize the next required action, current state, and clear progress feedback.
- The app must visually distinguish ready, measuring, completed, canceled, and failed states.
- The user needs explicit guardrails when a second measurement is blocked by an active session.
- The home screen must make the per-feature action model explicit so the user can choose between setting goals, viewing history, measuring, getting suggestions, and consulting professionals without ambiguity.
- Result screens must show the measurement outcome, progress relative to history, and recommended next action.
- Accessibility requirements include readable contrast, large tap targets, and clear non-color cues for state changes.
- The `consult professionals` flow must be visually distinct from measurement and suggestion flows so the user understands it is a support handoff rather than a device interaction.
- Factory mode must stay out of the consumer UX while still giving manufacturing and QA immediate pass/fail feedback through the LED and BLE log path.
- HW-ID must not appear in consumer-facing UI, but support and manufacturing tools need a consistent place to surface it for diagnosis.

## 11. Technical and Operational Considerations

- BLE reliability is a core dependency and must be validated under intermittent reconnect conditions.
- Cloud sync requires identity management and a retry strategy when offline.
- Subscription enforcement must be coordinated across mobile app, account backend, and any cloud-gated content.
- Third-party health integrations depend on external APIs, permission grants, and platform policy.
- Sensor accuracy, warm-up behavior, calibration, and sample consistency are central technical risks.
- The handheld enclosure and sample path must be co-designed with sensor selection, since airflow conditioning is now a first-order technical dependency rather than a cosmetic concern.
- The design should prefer passive airflow shaping and enclosure geometry over added moving parts so reliability, manufacturability, and cost remain aligned with the target.
- The one-action-at-a-time rule means the UI and firmware must coordinate state transitions carefully so users are never offered simultaneous conflicting actions, even when the app is backgrounded or the device is waking from low power.
- Low-power entry must not create a false failure state or drop a session context when sensor activity is merely idle.
- The `consult professionals` flow must not require additional PII, breath-data export, or payment state to function in Phase 1.
- Factory mode requires a dedicated one-time provisioning latch, BLE log transport, and LED state mapping so hardware validation can happen without opening the enclosure.
- HW-ID must be available to firmware, backend, analytics, manufacturing, and support systems as a routing key, but the consumer app should treat all supported hardware as the same product experience.
- Packaging, manufacturing, and cost targets must support the target retail price.
- The product must avoid presenting results as diagnostic unless approved by regulatory and legal review.

## 12. Success Metrics

### Launch KPIs

- 95% of pairing attempts complete successfully within 10 minutes on supported devices and OS versions, measured on rolling 30-day cohorts after launch.
- 70% of paired users complete their first measurement session within 24 hours of pairing, measured on rolling 30-day cohorts after launch.
- 35% of paired users complete at least 2 sessions within 30 days of pairing, measured on rolling 30-day cohorts after launch.
- 12% of users who reach trial expiration convert to paid within 14 days of expiration, measured on rolling 60-day trial cohorts.
- 98% of completed sessions sync to the cloud within 15 minutes when network connectivity is available, measured on rolling 7-day windows.
- 85% of transient disconnect events recover within 60 seconds without data loss, measured on rolling 30-day windows.
- 90% of paid or trial-active users complete a session without support intervention, measured on rolling 30-day windows.
- 90% of users can correctly identify the available action for a feature card on first exposure in usability testing, measured during pre-launch validation.
- 85% of low-power transitions recover to an actionable state within 5 seconds once user activity resumes, measured during device validation.
- 100% of production units record a completed Factory mode pass/fail outcome and HW-ID association before shipment, measured across manufacturing release lots.
- In controlled validation with at least 50 participants and 150 repeated-session pairs, 90% of consecutive same-source oral measurements remain within 5% variance within a 30-day validation window.
- In controlled validation with at least 50 participants and 150 repeated-session pairs, 90% of repeated Fat Burning readings from the same device/session pattern remain within 5% variance within a 30-day validation window.

### Supporting Telemetry

- Pairing funnel drop-off by step.
- Session start count by mode.
- Session cancelation count by cause.
- Baseline completion count for Oral & Dental Health.
- Average number of repeated readings per Fat Burning session.
- Recommendation open rate and tap-through rate.
- Apple Health export success rate.
- Health Connect export success rate.
- Entitlement-check-pending/offline incidence and recovery time.
- Subscription prompt view rate and post-prompt conversion rate.
- Factory pass/fail rate by HW-ID.
- Factory BLE log capture success rate.
- HW-ID routing coverage across manufacturing, backend, analytics, and support workflows.

## 13. Risks, Assumptions, and Open Questions

### Key risks

- Sensor accuracy may not meet the expected consistency target across environments or users.
- The app may need additional logic to prevent confusing state mismatches during disconnects.
- Subscription gating can create support issues if trial expiration is not clearly communicated.
- Third-party integration scope may vary by platform and partner API availability.
- Entitlement outages may create support burden if cached access and queueing rules are not transparent.
- The handheld industrial design may constrain sensor placement, airflow shaping, and battery volume enough to affect measurement accuracy or cost.
- Trying to achieve a slick enclosure with too many internal functions may create reliability or manufacturing risk if the team exceeds the preferred zero-moving-parts target.
- If airflow stabilization is not validated early, the product may appear to work in software while producing noisy or inconsistent breath-print data in real use.
- If the one-action-at-a-time behavior is not enforced consistently, the product may feel confusing or stateful in a way that undermines trust in the measurement results.
- Low-power behavior could create support issues if users interpret idle sensor reduction as a failure rather than a power-saving mode.
- Factory mode could be accidentally exposed to consumers if the one-time provisioning latch or long-press guard is not enforced consistently.
- HW-ID misclassification could cause support, analytics, or manufacturing reports to group the wrong hardware variant with a result or error log.
- BLE error-log transfer during Factory mode could fail and leave manufacturing without enough evidence to triage a bad unit unless logs are buffered.

### Assumptions

- The device can reliably report session and sensor state over BLE.
- The cloud backend exists to store history, goals, and subscription status.
- AI-assisted goal suggestions can be generated safely without claiming diagnosis.
- Subscription policy remains 60-day trial followed by $5.99 per month unless changed by business decision.
- The app can cache a verified entitlement for up to 24 hours when the backend is temporarily unavailable.
- A passive or minimally mechanical airflow-conditioning path can be achieved within the handheld industrial-design envelope and $199 retail target.
- The home-screen feature cards can support the same action vocabulary across modes without creating a heavy navigation hierarchy.
- Low-power mode can be implemented as a distinct state that preserves session context and resumes quickly when user activity returns.
- A curated directory plus external contact handoff is sufficient for `consult professionals` in Phase 1, with richer referral or booking workflows deferred to later phases.
- Factory mode can be consumed once per device and then locked out without requiring a consumer-facing reset path.
- Every supported hardware profile has a stable HW-ID mapping that downstream systems can use without changing the consumer measurement experience.

### Open questions

- What are the minimum acceptable retry rules for failed sessions?
- What guardrails are required for AI-generated goals and recommendations?
- Does the industrial design require any hygienic insert, removable mouth-contact component, or disposable accessory to satisfy airflow, sanitation, or comfort needs?
- What tolerance and calibration strategy is required for the stabilized airflow path to support the 5% same-source consistency target?
- What is the exact set of supported HW-ID values, and which firmware or backend components own the mapping table?
- How should manufacturing recover from a Factory-mode failure if BLE log transfer is unavailable on the first attempt?

## 14. Scope and Phased Delivery

### MVP / Phase 1

- BLE pairing and device readiness.
- Guided measurement for Oral & Dental Health and Fat Burning.
- Session completion, cancelation, and failure handling.
- Goal setup with optional AI-assisted suggestions.
- Feature cards and actions on the home screen for `set goals`, `view history`, `measure`, `get suggestion`, and `consult professionals`.
- Local and cloud history storage.
- Basic progress visualization.
- 60-day trial and paid subscription enforcement.
- Outbound sharing of completed session summaries to Apple Health on iOS and Health Connect on Android.
- Cached entitlement handling with read-only fallback and queued sync recovery.
- Low-power device behavior during sensor inactivity with fast resume to actionable states.
- Manufacturing-only Factory mode with one-time use, 3-color LED pass/fail signaling, and BLE error-log reporting.
- HW-ID capture and downstream routing for firmware, backend, analytics, manufacturing, and support use.
- Handheld industrial design with a compact enclosure, stable grip, and sensor/sample path layout that supports the approved measurement workflows.
- Passive airflow conditioning or an equivalently simple internal mechanism that prevents direct breath from hitting sensors.

### Later phases

- Expanded measurement modes beyond the first two use cases.
- Additional recommendation logic and richer personalization.
- Direct Oura, Fitbit, and similar partner integrations.
- More advanced longitudinal analytics and comparison views.
- Optional refinement of enclosure serviceability or hygiene accessories if Phase 1 validation shows the handheld sample path needs them.
- Consumer-visible Factory mode controls or HW-ID display are not planned for later phases and remain internal-only.

### Excluded from Phase 1

- Additional health categories beyond oral/dental and fat-burning.
- In-app commerce checkout flows.
- Clinical diagnosis or medical decision support.
- Multi-user concurrent device sessions.
