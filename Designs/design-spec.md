# AirHealth Design Specification

## 1. Overview

- Feature name: AirHealth connected breath analysis experience
- Design objective: Define a Figma-ready product experience for pairing, feature-card task selection, guided breath measurement, results review, progress tracking, entitlement states, low-power behavior, support-directory access, manufacturing verification, and platform sharing across the two approved modes.
- Source inputs used: Approved PRD in `PM/PRD/PRD.md`, feature definition in `PM/Designs/feature.md` (2026-03-27 update)
- Summary of the user experience being designed: A user pairs a handheld breath-analysis device to a mobile app, selects one of two modes, follows guided measurement instructions, reviews a normalized result and progress over time, and manages trial/subscription and sharing states when applicable. Separate internal surfaces support Factory verification and HW-ID-based routing without exposing those details to consumers.

## 2. Inputs and Alignment

### Relevant PRD inputs

- The product has two initial modes: Oral & Dental Health and Fat Burning.
- The user can only run one measurement session at a time.
- The user can only run one action at a time across measurement, goal editing, suggestion generation, history mutation, and support entry.
- The mobile app is the primary experience surface.
- Phase 1 sharing is limited to Apple Health on iOS and Health Connect on Android.
- The system must handle paired, disconnected, ready, active session, complete, canceled, failed, trial active, paid active, expired/read-only, and entitlement-check-pending/offline states.
- The app must support a 60-day trial followed by a $5.99 monthly subscription.
- The device is a handheld product with a compact enclosure, minimal moving parts, and an airflow path that conditions the sample before it reaches the sensors.
- The home screen presents each feature as a task hub with `set goals`, `view history`, `measure`, `get suggestion`, and `consult professionals`.
- `consult professionals` is a Phase 1 external educational and support directory for both features.
- The device enters low-power mode only after 3 consecutive seconds below the 1% idle threshold and exits low-power mode only after explicit user/app action or 2 consecutive seconds above the exit threshold.
- The product also includes a 3-color LED for device-state feedback.
- Factory mode exists for manufacturing and QA, is one-time use, and is not exposed to consumer UX.
- HW-ID is internal routing metadata for manufacturing, backend, analytics, and support, and must not surface in consumer UX.

### Relevant inputs from `PM/Designs/feature.md`

- Users deep-breathe or mouth-breathe according to app instructions.
- Oral & Dental Health uses hydrogen sulfide and methyl mercaptan sensors.
- Fat Burning uses acetone and CO2 sensors.
- Oral mode normalizes to the average of the first 5 measurements.
- Fat mode normalizes to the session start point and supports repeated readings in one session.
- If a measurement is interrupted before completion, it is canceled and not stored.
- Device interaction is limited to power on/off; the app provides instructions.
- The physical device should stay as slick as possible, with as few mechanical parts as possible.
- Airflow must be stabilized before reaching the sensors and must not be blown directly onto the sensors.

### Assumptions made for design

- The device has no consumer-facing display in Phase 1; the app carries the instructional and status burden.
- The device may use simple non-screen indicators internally, but the design does not require them.
- The primary app structure is bottom navigation with Home, History, Goals, Sharing, and Account/Billing.
- AI-assisted goal suggestions are presented as optional guidance, not as diagnosis or medical advice.
- Recommendation content is framed as wellness guidance and can link out, but checkout remains outside the product.
- The handheld industrial design will likely use a fixed mouth-contact or air-intake region rather than a complex mechanical assembly, unless later engineering validation proves a removable hygienic part is required.
- If a removable hygienic part is introduced later, it should be passive and user-replaceable, not a moving or actuated mechanism.
- The feature-card action row is visible on the feature detail surface and may also be pinned on Home when space allows, but the same action vocabulary must remain consistent in both places.
- Factory verification will likely use a separate internal tool or service surface rather than the consumer app, even though the underlying device hardware is shared.

### Unresolved ambiguities that affect design

- The PRD defines the result models but does not prescribe a branded visual language for scores and progress. The design must therefore use clear numeric labels and simple trend graphics.
- The PRD allows cached entitlement behavior during backend unavailability, but the exact backend error copy remains a product content decision.
- The PRD defines export payload fields, but partner-specific UI permissions and copy still need final legal/privacy review before release.
- The PRD intentionally leaves open whether the handheld air path needs a removable hygienic insert or disposable accessory.
- The PRD defines the support-directory behavior, but the exact content source, taxonomy, and locale coverage of the curated directory still need operational definition.
- The PRD defines HW-ID as internal routing metadata, but the exact values, taxonomy, and owning service remain implementation decisions.
- The PRD does not prescribe the final consumer LED mapping outside Factory mode, so consumer LED semantics may need a later hardware/state spec update.

## 3. Experience Architecture

### Entry points

- First launch onboarding
- Device pairing flow
- Home screen feature-card hub
- History and trends
- Goals setup and edit
- Suggestion request and response
- Consult professionals directory
- Factory verification console
- Subscription/paywall surfaces
- Sharing settings

### Major flows

- Pair device and complete first setup
- Select a feature and choose an action
- Set a goal from the feature hub
- Perform Oral & Dental Health measurement
- Perform Fat Burning measurement
- Review results and progress
- Open `consult professionals` and hand off to an external resource
- Run factory verification and capture pass/fail logs
- Handle trial, entitlement, and read-only states
- Enter and exit low-power readiness without losing context
- Share completed sessions to Apple Health or Health Connect
- Review handheld device shell, grip, button, and sample-path experience

### Key surfaces

- Mobile app onboarding screens
- Home/dashboard
- Pairing sheet and device discovery
- Feature detail/task hub and goal setup
- Guided measurement screen
- Result summary screen
- History detail and trend screen
- Suggestion surface
- Consult professionals directory and external handoff notice
- Factory verification screen and BLE log viewer
- Subscription state screen
- Sharing permissions and export summary screen
- Error and recovery dialogs

### Touchpoints between hardware and software

- Device power button triggers powered-off and powered-on states.
- BLE pairing and reconnect state are shown in-app.
- Session start, live measurement, completion, cancellation, and failure are driven by device/app state sync.
- The app is authoritative for instructions, result presentation, and entitlement-gated actions.
- The app is also authoritative for the feature-card action hub and for disabling conflicting actions while one action is active.
- The device enclosure and sample path must make the handheld experience feel stable and obvious, while still protecting sensor accuracy through airflow conditioning.
- The device firmware is authoritative for low-power entry and exit, while the app must communicate that state without implying failure.
- Factory verification is authoritative on the device and internal tooling, and it may bypass consumer sign-in while still writing logs to authorized systems.
- The 3-color LED is secondary for consumer use but primary for factory pass/fail and in-progress verification states.

### Critical system states

- Unpaired
- Paired but disconnected
- Paired and ready
- Feature hub active
- Active session
- Measuring
- Low power
- Factory mode
- Factory check running
- Factory pass
- Factory fail
- Factory locked
- Complete
- Canceled
- Failed
- Trial active
- Paid active
- Expired/read-only
- Temporary access
- Read-only mode

### User-visible transitions

- Unpaired to paired after successful BLE handshake
- Paired and ready to feature hub after successful setup
- Feature hub to measuring after the user chooses `measure`
- Feature hub to history, goals, suggestion, or support after the user chooses a single action
- Measuring to complete after device confirms a valid sample
- Measuring to canceled after user aborts or exits
- Measuring to failed after disconnect, invalid sample, or sensor issue
- Ready to low power after the firmware idle threshold is met
- Low power to ready after explicit wake or above-threshold sensor activity
- Powered on to factory check running after a 10-second factory long-press by authorized tooling
- Factory check running to factory pass when the hardware check completes without error
- Factory check running to factory fail when the hardware check reports one or more errors
- Factory pass or factory fail to factory locked after provisioning is complete
- Factory locked to powered off after the exit long-press or equivalent shutdown step
- Trial active or paid active to expired/read-only after entitlement lapses
- Temporary access to read-only mode after the cached entitlement window becomes stale
- Handheld setup to measurement-ready after the user powers on the device, grips it naturally, and the app confirms the form factor is ready for a guided session

## 4. User Flows

### Flow 1: First-time setup and pairing

- User goal: Connect the device and get to a state where measurement is possible.
- Preconditions: App installed, device powered on, Bluetooth permission available or requestable.
- Trigger: First launch CTA, or explicit `Add device` action from Home.
- Steps in sequence:
  1. User launches app and sees a welcome screen with the two modes and trial framing.
  2. App requests Bluetooth permission with a short explanation of why pairing is needed.
  3. User enters pairing flow and discovers the nearby device.
  4. App confirms device identity at a high level using model name rather than serial number.
  5. App shows paired and ready state, then prompts mode selection.
  6. User chooses a mode and sets or accepts a suggested goal.
- Expected system responses: Discovery, pairing progress, success confirmation, and a clear next step into mode setup.
- Exit states: Paired and ready, with one mode configured.
- Alternate paths: Permission denied, device not found, wrong device, pairing timeout.
- Recovery paths: Retry discovery, re-request permission, go back to pairing, or defer setup and continue as read-only if entitlement state requires it.

### Flow 2: Manufacturing verification

- User goal: Verify the device hardware, capture logs, and confirm pass/fail before consumer shipment.
- Preconditions: Authorized factory operator or QA technician, unprovisioned or factory-eligible device, BLE capture tooling available.
- Trigger: 10-second hardware long-press or manufacturing tooling command.
- Steps in sequence:
  1. Authorized personnel initiate Factory mode on the device.
  2. Device enters factory check running state and turns the 3-color LED orange.
  3. Firmware runs the one-time hardware functionality check and emits BLE logs.
  4. Device turns LED green on pass or red on fail.
  5. Factory tooling records the result and associated HW-ID metadata.
  6. Device exits to locked state and does not allow re-entry after provisioning.
- Expected system responses: The hardware check completes once per device, logs are available over BLE, and the consumer app is not involved.
- Exit states: Factory pass, factory fail, factory locked.
- Alternate paths: BLE log transfer interrupted, long-press not recognized, already provisioned device, missing HW-ID mapping.
- Recovery paths: Retry log capture if the device remains in factory check state, or quarantine the unit for manual triage if the device is already locked or failed.

### Flow 3: Feature-card task hub

- User goal: Choose the right next action from a feature without navigating through unrelated settings.
- Preconditions: Paired device or accessible read-only history state, Home visible, at least one feature card available.
- Trigger: User taps a feature card from Home.
- Steps in sequence:
  1. User lands on the feature detail or expanded feature card surface.
  2. App shows the action set for that feature: `set goals`, `view history`, `measure`, `get suggestion`, and `consult professionals`.
  3. App enables only actions allowed in the current entitlement, device, and session state.
  4. User chooses one action and the rest of the action row becomes disabled until that flow resolves.
- Expected system responses: The user always sees one consistent action vocabulary, and the app never presents simultaneous conflicting actions as available.
- Exit states: Goal edit flow, measurement flow, history flow, suggestion flow, support-directory flow, or blocked state with explanation.
- Alternate paths: Read-only mode, temporary access, disconnected device, active session already in progress.
- Recovery paths: Refresh device status, return to Home, or choose a non-blocked action.

### Flow 4: Oral & Dental Health measurement

- User goal: Capture a single oral session and understand the result relative to baseline.
- Preconditions: Paired device, oral mode selected, device ready, user has an eligible entitlement state.
- Trigger: `Measure now` from Home or Oral mode card.
- Steps in sequence:
  1. App shows a preparation screen with a deep-breath prompt and mouth placement animation.
  2. User starts the session and the app enters measuring state.
  3. The app shows a live progress indicator and a clear instruction to hold the device in the mouth and close the mouth.
  4. Device samples until valid completion is reached.
  5. App confirms completion and presents the Oral Health Score, baseline reference, and progress.
  6. User can save, share, or view history.
- Expected system responses: Clear phase changes, no ambiguous partial results, and storage only after valid completion.
- Exit states: Complete, canceled, or failed.
- Alternate paths: User stops early, device disconnects, sensor error, cached entitlement blocks a new start.
- Recovery paths: Retry after error, reconnect device, or resume as read-only if the session cannot start.

### Flow 5: Fat Burning measurement

- User goal: Capture repeated readings in one session and see whether progress is moving toward a target.
- Preconditions: Paired device, fat mode selected, device ready, valid entitlement state.
- Trigger: `Measure now` from Home or Fat mode card.
- Steps in sequence:
  1. App shows a preparation screen with the 10-second hold and blow instructions.
  2. User starts session and sees a live step-by-step coach.
  3. User completes a reading and sees the current delta and best delta so far.
  4. User repeats readings until target or self-defined end condition is reached.
  5. User taps `Finish`.
  6. App shows session summary, best delta, goal target, and goal achieved state.
- Expected system responses: The app keeps current and best values distinct and prevents cross-session comparison of interim points.
- Exit states: Complete, canceled, or failed.
- Alternate paths: User ends early, device disconnects, sample invalid, or target not set.
- Recovery paths: Reconnect and retry if the session has not been finalized, or return to Home if the session is lost.

### Flow 6: Consult professionals

- User goal: Find relevant external educational or professional support resources without leaving the product confused about what will be shared.
- Preconditions: User is on a feature card, result screen, or history detail screen; no conflicting action is active.
- Trigger: User taps `consult professionals`.
- Steps in sequence:
  1. App opens a feature-specific directory with curated links, phone numbers, and support hours.
  2. App labels the flow as informational support and explains that no breath, result, or account data will be transmitted.
  3. User selects an external destination.
  4. App shows an external-handoff notice before opening the destination.
- Expected system responses: The support directory feels distinct from measurement and suggestion flows, and the handoff is explicit before the user leaves AirHealth.
- Exit states: External handoff complete, support directory dismissed, or external destination unavailable.
- Alternate paths: Read-only mode, temporary access, missing locale-specific content, blocked because another action is already active.
- Recovery paths: Return to the feature hub, retry with another resource, or open generic non-localized support content if no localized match exists.

### Flow 7: Trial, entitlement, and read-only behavior

- User goal: Understand whether they can measure now, view history, or only review past data.
- Preconditions: Account exists and the app can verify or cache entitlement state.
- Trigger: App launch, session attempt, or settings visit after trial expiration.
- Steps in sequence:
  1. App determines entitlement state.
  2. If entitlement is active, normal actions remain enabled.
  3. If verification is unavailable but a fresh cached entitlement exists, the app shows `Temporary access`.
  4. If entitlement is expired or cached state is stale, the app shows `Read-only mode`.
  5. User sees explicit messaging about whether new sessions are blocked.
- Expected system responses: Clear state labeling, action gating, and retention of synced history.
- Exit states: Trial active, paid active, temporary access, or read-only mode.
- Alternate paths: Backend unavailable, offline launch, expired subscription, payment issue.
- Recovery paths: Refresh entitlement, sign in again, or restore paid status.

### Flow 8: Share completed session summary

- User goal: Export a completed result to Apple Health or Health Connect.
- Preconditions: Session is complete, destination permission is granted or requestable.
- Trigger: `Share to Health` action on result or settings screen.
- Steps in sequence:
  1. App explains what will be shared and what is not shared.
  2. User approves platform permissions.
  3. App sends the completed summary payload only.
  4. App confirms success or surfaces a platform-specific failure.
- Expected system responses: Only completed sessions are exportable, and no raw sensor data leaves the app.
- Exit states: Export success, export failed, permission denied.
- Alternate paths: Unsupported platform, partial permission, sync pending.
- Recovery paths: Retry export, return to sharing settings, or dismiss without losing the local result.

### Flow 9: Low-power readiness and wake

- User goal: Understand that the device is idle but still healthy, and resume quickly without losing context.
- Preconditions: Device paired, no measurement transition in flight, sensor readings effectively idle.
- Trigger: Firmware crosses the 3-second below-threshold idle rule, or a wake event occurs from user/app action or renewed sensor activity.
- Steps in sequence:
  1. Device enters low power after the idle threshold is satisfied.
  2. App shows a low-power-ready state rather than an error state.
  3. User taps a feature action or the device senses above-threshold activity.
  4. Firmware exits low power using the hysteresis rule and the app returns to ready or measuring.
- Expected system responses: The product never chatters between ready and low power, and no wake transition looks like a disconnect or failure.
- Exit states: Ready, measuring, or low-power-ready.
- Alternate paths: Noisy sensor readings near threshold, wake without valid entitlement, wake during reconnect.
- Recovery paths: Retry wake, reconnect device, or show explanatory low-power help text.

## 5. Screen and Interaction Specification

### Welcome / onboarding

- Purpose: Explain what the product does and set expectations about trial, device pairing, and two modes.
- Content requirements: Product value statement, brief setup steps, trial messaging, and a clear `Get started` CTA.
- Controls and actions: `Get started`, `Sign in`, `Learn more`.
- Information hierarchy: Value proposition first, setup steps second, legal/trial details last.
- User feedback: Immediate transition into permission and pairing flow after CTA.
- State behavior: If entitlement is already known, the app may skip parts of onboarding.
- Dependencies: Account and entitlement state may change what is shown.
- Accessibility considerations: Large primary button, plain-language copy, and no reliance on color alone.

### Factory verification console

- Purpose: Let authorized manufacturing and QA users run the one-time hardware check and capture pass/fail evidence without consumer UI.
- Content requirements: Device provisioning status, factory start control, live hardware-check status, 3-color LED state legend, BLE log capture panel, HW-ID routing metadata, pass/fail summary.
- Controls and actions: Start check, copy logs, retry capture, mark for quarantine, exit factory.
- Information hierarchy: Device identity and provisioning state first, check state second, logs and routing metadata third.
- User feedback: Orange while checking, green on pass, red on fail, with the BLE log stream clearly associated to the same device instance.
- State behavior: The console must be unavailable to consumer accounts and hidden from consumer app navigation.
- Dependencies: Factory tooling, BLE logging path, provisioning state, HW-ID mapping service.
- Accessibility considerations: High-contrast pass/fail states and text labels for the LED colors.

### Pairing screen

- Purpose: Find the device and establish BLE connection.
- Content requirements: Nearby device list, discovery state, pairing progress, retry and help copy.
- Controls and actions: Select device, retry scan, cancel.
- Information hierarchy: Device identity and pairing status above secondary help text.
- User feedback: Loading state during discovery, success confirmation, and device-connected state.
- State behavior: Permissions denied or no device found must surface a concrete next action.
- Dependencies: Bluetooth permissions, device power, proximity, and OS availability.
- Accessibility considerations: Announce pairing status changes and keep retry controls reachable.

### Home dashboard

- Purpose: Give a single place to select a feature and then choose one allowed action from that feature's task hub.
- Content requirements: Connected device status, entitlement state, mode cards, action row or action sheet, last result preview, next recommended action, and low-power-ready status when applicable.
- Controls and actions: Open feature hub, `set goals`, `view history`, `measure`, `get suggestion`, `consult professionals`, manage subscription.
- Information hierarchy: Current readiness, then feature/action availability, then recent progress.
- User feedback: Disabled or deferred actions must explain whether the block comes from entitlement, device state, low-power wake, or one-action-at-a-time locking.
- State behavior: Home must reflect ready, disconnected, low-power-ready, temporary access, and read-only states without ambiguity.
- Dependencies: Device state, entitlement state, cached history.
- Accessibility considerations: Clear labels for disabled actions and focus order that follows task priority.
- LED behavior: Any consumer-facing LED cue should be redundant to the app; the app copy should not require the user to infer meaning from color alone.

### Mode selection and goal setup

- Purpose: Let the user choose Oral & Dental Health or Fat Burning and establish a target.
- Content requirements: Mode descriptions, brief explanation of what the score means, optional AI-suggested goal, manual edit controls.
- Controls and actions: Select mode, accept suggestion, edit goal, save.
- Information hierarchy: Mode purpose, then measurement behavior, then goal entry.
- User feedback: Confirmation when the mode becomes active and goal saved.
- State behavior: Only one active mode at a time; switching requires session completion or cancellation.
- Dependencies: Existing session state and account entitlement.
- Accessibility considerations: Avoid jargon and keep numeric targets editable with clear bounds.

### Guided measurement screen

- Purpose: Coach the user through the live measurement without requiring interpretation of raw sensor data.
- Content requirements: Step card, animated instruction, live state label, current step indicator, cancel action.
- Controls and actions: Cancel, finish where appropriate, retry after recoverable error.
- Information hierarchy: Next action first, live status second, detail text third.
- User feedback: Explicit transitions between `ready`, `measuring`, `processing`, and `complete`.
- State behavior: No screen should imply completion before device confirmation.
- Dependencies: Device sensor state, BLE connection, and session type.
- Accessibility considerations: Non-color state labels, short instruction copy, and reduced motion support.

### Result summary screen

- Purpose: Show the completed score, progress, and recommended next step.
- Content requirements: Primary result value, baseline or target reference, trend sparkline, goal achieved status, sharing action, history link, and the same feature-specific next-action choices the user can take after reviewing the result.
- Controls and actions: Share, save, view trend details, retake if allowed, `get suggestion`, `consult professionals`.
- Information hierarchy: Result first, then progress, then actions.
- User feedback: Result only appears after validated completion.
- State behavior: Oral and fat results use different semantics but share a consistent structure, and the available next actions must return the user to the same feature context they started from.
- Dependencies: Session type, stored history, entitlement status.
- Accessibility considerations: Numeric results must have text labels in addition to charts.

### History and trend detail

- Purpose: Let the user see long-term progression and prior sessions.
- Content requirements: Timeline, filters by mode, baseline marker, summary cards, and support-oriented next actions when relevant to the selected feature.
- Controls and actions: Change range, open session detail, export/share from completed sessions, `consult professionals`.
- Information hierarchy: Latest result and trend trendline first, drill-down second.
- User feedback: Read-only history remains visible even in temporary access or read-only mode.
- State behavior: No editable controls in read-only mode.
- Dependencies: Cloud sync and local cache.
- Accessibility considerations: Trend charts require textual summary of change over time.

### Subscription and entitlement surfaces

- Purpose: Explain trial, paid, temporary access, and read-only conditions.
- Content requirements: Current status, expiration timing, what is available now, what is blocked, and reactivation CTA.
- Controls and actions: Renew, restore purchase, refresh entitlement, dismiss.
- Information hierarchy: Current access state first, consequences second, recovery action third.
- User feedback: State changes must be explicit and not hidden behind generic paywall language.
- State behavior: Cached access is clearly labeled as temporary when applicable.
- Dependencies: Backend entitlement service and platform billing state.
- Accessibility considerations: Avoid legalese in the primary message and keep CTA labels direct.

### Sharing settings

- Purpose: Let the user export completed summaries to Apple Health or Health Connect.
- Content requirements: Destination list, what is shared, what is excluded, permission status, last export status.
- Controls and actions: Connect, disconnect, retry export.
- Information hierarchy: Permission status first, supported destinations second, field disclosure last.
- User feedback: Clear success or failure after export attempt.
- State behavior: Unsupported fields are omitted, never approximated.
- Dependencies: Platform permissions and supported destination schema.
- Accessibility considerations: Use simple language for data-sharing disclosure.

### Factory verification screen

- Purpose: Display the status of a one-time hardware check and log transfer for internal manufacturing or QA use.
- Content requirements: Check progress, pass/fail status, 3-color LED legend, BLE log capture state, HW-ID routing metadata, provisioning lock status.
- Controls and actions: Start check, stop capture, export logs, quarantine unit, lock device.
- Information hierarchy: Pass/fail result first, logs and routing metadata second, maintenance actions third.
- User feedback: Orange during checking, green on pass, red on fail, with the BLE log state visibly tied to the device under test.
- State behavior: The screen must not be reachable from consumer app navigation and must disappear once the device is factory-locked.
- Dependencies: Factory provisioning, BLE logs, HW-ID mapping, internal operator credentials.
- Accessibility considerations: Text labels for every status color and no reliance on color alone.

### Consult professionals directory

- Purpose: Provide a safe, clearly external support directory that helps users find relevant educational and professional resources by feature.
- Content requirements: Feature-specific directory list, resource type, short description, support hours if known, locale filtering message, and explicit `no health data shared` notice.
- Controls and actions: Open resource, return to feature, filter if available.
- Information hierarchy: Safety/context notice first, recommended resources second, secondary metadata third.
- User feedback: External-handoff notice before any external website, phone app, or contact method opens.
- State behavior: Available in trial active, paid active, temporary access, and read-only states; disabled during active measurement or any conflicting in-progress action.
- Dependencies: Feature context, locale availability, external destination health.
- Accessibility considerations: Resources must be scannable, with link purpose clearly labeled for screen readers.

### HW-ID routing and logs

- Purpose: Let internal systems associate results and factory logs with the correct hardware profile and detected VOC type.
- Content requirements: Internal HW-ID value, supported hardware profile, routing destination, factory log association, and any status that indicates the mapping is available.
- Controls and actions: None for consumers; internal tooling may copy, export, or filter by HW-ID.
- Information hierarchy: Hidden from consumer UI; exposed only in authorized internal tools.
- User feedback: None in consumer surfaces; internal tools should show a clear error if the HW-ID mapping is missing or inconsistent.
- State behavior: HW-ID never changes consumer measurement behavior, but it may affect manufacturing, backend, analytics, and support grouping.
- Dependencies: Firmware, backend mapping table, manufacturing tooling, analytics pipelines, support tooling.
- Accessibility considerations: Internal tooling should label HW-ID with a human-readable hardware profile name as well as the raw key.

## 6. States and Conditions

### Default states

- Unpaired: The user sees onboarding and pairing CTAs.
- Paired, ready: The user sees mode cards and a measurement entry point.

### Active states

- Feature action in progress: The chosen action owns the UI and the rest of the feature actions are disabled.
- Measuring oral: The app shows oral instructions and progress.
- Measuring fat: The app shows the repeated-reading coach and current/best delta.
- Factory check running: Internal tooling shows orange LED and logs in progress.
- Factory pass: Internal tooling shows green LED and a successful hardware-check result.
- Factory fail: Internal tooling shows red LED and a failed hardware-check result.

Factory-related states are internal-only and must not appear in consumer-facing screens, settings, or exports.

### Loading states

- Device discovery scanning
- Entitlement verification
- Cloud sync after session completion
- Export to health platforms
- Low-power wake handshake
- BLE log capture during factory verification

### Connected and disconnected states

- Connected: Full app controls are enabled subject to entitlement.
- Disconnected: App shows reconnect CTA and preserves local state where possible.

### Success states

- Session complete
- Baseline established
- Export succeeded
- Subscription restored
- Factory pass
- Factory fail
- Factory locked

### Warning states

- Temporary access
- Low-power ready
- Battery low before session start
- Goal not set
- Factory mode in progress

### Error states

- Pairing failed
- Sensor warm-up failed
- Invalid sample
- Device disconnected mid-session
- Export failed
- External support resource unavailable
- Entitlement unavailable
- Factory log capture failed
- Factory check failed
- HW-ID mapping missing

### Recovery states

- Retry pairing
- Reconnect device
- Retry export
- Wake from low power
- Refresh entitlement
- Retry BLE log capture
- Quarantine unit
- Retry factory log capture

### Unsupported states

- No supported share destination on the current platform
- Read-only mode
- Expired entitlement without payment

For each state, the UI must show what happened, what is blocked, and the next recoverable action if one exists.

## 7. Edge Cases and Failure Handling

- Failed setup: Keep onboarding resumable and avoid forcing a full restart after permission denial.
- Failed pairing: Show a short reason, a retry button, and a way to return to device discovery.
- Device offline: Preserve the last known state and show reconnect instructions.
- App offline: Permit viewing synced history, cache local results, and delay uploads.
- Partial completion: Never store a partial session as complete, and never show a result card as final.
- Interrupted flows: If the user closes the app during a live measurement, resume only if the device still reports the same active session ID.
- One-action conflict: If the user tries to launch another task from the feature hub while one action is active, explain which action is in progress and what must finish or be canceled first.
- Permission denial: Explain why Bluetooth or health export permissions are needed and offer a retry path.
- Firmware mismatch: Block measurement start and explain that device software must be updated before use if the device reports incompatibility.
- Account mismatch: Warn clearly when the signed-in account differs from the account that owns synced history or subscription access.
- Unavailable hardware: If the required sensor set is unavailable or reports failure, block the mode and preserve history access.
- Low-power chatter risk: Use the hysteresis rule so the UI does not flicker between ready and low-power-ready when readings hover near the threshold.
- External support outage: If a professional-support destination fails to open, keep the user in AirHealth and offer another resource rather than dropping them into a dead-end state.
- Factory lockout issue: If a unit fails provisioning or remains factory-eligible after shipment, consumer UX must still hide Factory mode and surface only a supportable device error path.
- HW-ID mismatch: If the internal HW-ID cannot be mapped, internal tooling should flag the record without changing consumer measurement flows.
- 3-color LED ambiguity: If the LED disagrees with the app on a consumer device, the app copy must win and the LED should be treated as secondary until synchronized.
- Factory LED/log mismatch: If factory LEDs and BLE logs disagree, internal tooling should prioritize the captured log result for triage and mark the unit for quarantine.
- Factory re-entry attempt: If an already provisioned unit receives the long-press gesture, the device must remain out of Factory mode and route only to internal supportable failure handling.
- Missing HW-ID on a factory record: The record should remain visible to internal tooling for follow-up, but it must not affect consumer setup, measurement, or history access.
- Timeout and retry behavior: Offer one explicit retry path before returning to Home.
- State mismatch between app and device: Treat the device as authoritative for live session result, and the app as authoritative for UI recovery and queued sync.

## 8. Feature Specifications

### Handheld device and airflow path

- Objective: Define the physical product experience so the device is comfortable to hold, simple to manufacture, and able to produce stable samples for both modes.
- User value: The user can hold and use the device naturally without managing a complex mechanical assembly or wondering whether breath placement will affect measurement quality.
- Design scope: Industrial design, enclosure geometry, mouth-contact region, airflow path, button placement, LED placement, and any visible indicator surfaces that support the handheld experience.
- Relevant PRD requirements: Handheld form factor, minimal mechanical parts, airflow stabilization before the sensors, no direct breath impact on sensors, $199 retail target.
- Hardware touchpoints: Device shell, grip area, power button, mouth-contact region, 3-color LED, internal sample-conditioning path, sensor chamber.
- Software touchpoints: Device readiness state, low-power-ready state, measurement start instructions, live measurement progress, error messaging when airflow or warm-up is not ready.
- Interaction spec: The design should make the user path obvious: hold, power on, follow the app, place mouth or blow into the intake region, and rely on the device to condition airflow before sensing. No flow should require the user to manipulate internal parts. The LED should provide redundant state feedback rather than forcing the user to infer meaning from color alone.
- State and behavior spec: The device should feel ready when handheld, visibly state when it is measuring or warming up, and clearly reflect when a sample is invalid because the breath path or readiness state is not within spec. The intake orientation should be visually and tactilely obvious so the user does not aim breath directly at exposed sensors. Factory mode LED behavior must be orange during check, green on pass, and red on fail.
- Edge cases: Grip instability, blocked airflow inlet, wet or obstructed mouth-contact area, sensor warm-up delay, sample inconsistency caused by direct breath impingement, passive hygienic accessory not seated if one is introduced later, consumer-device LED mismatch, factory-mode misuse attempt.
- Acceptance criteria: The user can hold the device comfortably during the full measurement flow, the measurement path is understandable without exposing sensors directly, and the design does not depend on moving parts to guide breath into the sensors.
- Success metrics: Handheld setup comprehension, measurement initiation success, airflow-related failure rate, low-power wake comprehension, user comfort feedback, reduction in support contacts related to device handling, and successful recognition of factory LED pass/fail behavior in internal validation.

### Factory mode

- Objective: Provide a controlled one-time manufacturing and QA flow for open-box verification and hardware triage.
- User value: Authorized factory and QA users can verify a unit, capture logs, and confirm whether the device is ready before shipment.
- Design scope: Internal factory start/exit flow, pass/fail indication, BLE log capture, quarantine/lockout behavior, and no consumer exposure.
- Relevant PRD requirements: Manufacturing-only Factory mode, 10-second long-press entry/exit, 3-color LED orange/green/red behavior, BLE error logs, one-time use, factory lockout, no consumer UI exposure.
- Hardware touchpoints: Button long-press, 3-color LED, BLE log transport, provisioning lock, HW-ID metadata.
- Software touchpoints: Internal factory console, log viewer, support/manufacturing tooling, quarantine status, lockout state.
- Interaction spec: The internal factory surface should show a single explicit start action, a visible in-progress state, a pass/fail outcome, and a lockout indicator after provisioning. The consumer app must never present this flow, and any consumer-facing support messaging should stop at a generic device-failure explanation rather than exposing factory controls.
- State and behavior spec: The device accepts a 10-second long-press into Factory mode only before consumer provisioning, runs exactly one check, emits BLE logs, and then becomes factory-locked. Orange indicates the check is running, green indicates pass, and red indicates fail. After provisioning, the same long-press must not re-open Factory mode.
- Edge cases: Long-press not recognized, BLE log transfer interrupted, unit already factory-locked, hardware failure during check, factory operator disconnect, mismatch between LED and log result, HW-ID missing from a factory record.
- Acceptance criteria: Internal users can start a factory check once, see a pass/fail result, capture BLE logs, and confirm the unit is locked out from re-entry after provisioning. Consumer surfaces never reveal Factory entry, Factory status, or manufacturing logs.
- Success metrics: Factory pass/fail capture rate, BLE log capture success rate, time-to-triage for failed units, percentage of units successfully locked before consumer shipment, and zero consumer exposure incidents for Factory controls.

### HW-ID routing and logs

- Objective: Define how internal hardware identifiers support manufacturing, analytics, backend routing, and support workflows without becoming a consumer feature.
- User value: Internal teams can group logs and results by hardware profile and detected VOC type while consumers see a single consistent product experience.
- Design scope: Internal routing labels, support/manufacturing views, log metadata display, and error states when mapping is missing.
- Relevant PRD requirements: HW-ID is internal-only, not consumer-facing, and is used to route outputs by supported hardware profile and detected VOC type.
- Hardware touchpoints: Firmware metadata emission, BLE logs, backend mapping table.
- Software touchpoints: Manufacturing console, analytics pipelines, support tools, backend ingestion.
- Interaction spec: HW-ID should only appear in internal tools and be shown alongside a readable hardware profile name. The consumer app must not ask for it, display it, or require it to complete any consumer flow.
- State and behavior spec: If HW-ID is missing or unmapped, internal systems should flag the record and keep consumer measurement flows unchanged.
- Edge cases: Unsupported HW-ID, detected VOC profile mismatch, log association failure, support lookup by the wrong device variant.
- Acceptance criteria: Internal tooling can associate a factory log or completed session with the correct hardware profile, and consumers never see HW-ID in UI. The same HW-ID should not change the consumer action model or the on-device measurement flow.
- Success metrics: HW-ID routing coverage, mapping success rate, support-case resolution time by hardware profile, and zero consumer-surface leakage of HW-ID values.

### Feature action hub

- Objective: Make each feature card act like a predictable task hub instead of a loose collection of separate screens.
- User value: Users can choose their next task quickly from the same mental model regardless of feature mode.
- Design scope: Home card, expanded feature surface, result-return actions, blocked-action states, and one-action-at-a-time locking.
- Relevant PRD requirements: Home-screen feature cards, task-hub actions for goals/history/measure/suggestion/support, and one-action-at-a-time enforcement.
- Hardware touchpoints: Device readiness, low-power-ready indication, active-session lock.
- Software touchpoints: Home, feature detail, result summary, history detail, entitlement gating, action locking.
- Interaction spec: The same action vocabulary must appear in a consistent order, with one primary action visually emphasized according to current state. When one action starts, the rest become visibly unavailable rather than silently disappearing.
- State and behavior spec: Ready state shows the full action set; active measurement shows only the in-progress measurement context; read-only and temporary-access states preserve history and support actions while blocking measurement and goal edits as required by entitlement.
- Edge cases: Conflicting actions, disconnected device, stale entitlement, low-power wake delay, no history yet.
- Acceptance criteria: Users can identify the allowed action set for a feature, understand why blocked actions are unavailable, and return to the same feature context after completing a task.
- Success metrics: First-try action selection accuracy, blocked-action comprehension, and action-to-completion rate by feature.

### Oral & Dental Health

- Objective: Help users measure and track oral and dental-related breath indicators over time.
- User value: A simple way to see progress and baseline shifts in a repeatable oral routine.
- Design scope: Setup, guidance, baseline-building, results, history, and error handling for oral sessions.
- Relevant PRD requirements: Hydrogen sulfide and methyl mercaptan sensors, first 5 sessions establish baseline, score normalized to a 0-100 scale.
- Hardware touchpoints: Handheld enclosure, device power button, BLE state, sensor readiness, airflow-conditioning path.
- Software touchpoints: Mode selection, guided measurement, score visualization, baseline history, recommendation surface.
- Interaction spec: The app should show a preparatory instruction screen, a single live measurement state, and a completion summary that clearly labels baseline-building versus steady-state results. The physical device should support a stable mouth placement experience without exposing the sensors directly.
- State and behavior spec: During the first 5 valid sessions, show `Baseline building` and a progress fraction. After baseline locks, show the score relative to the baseline line and make the score direction obvious.
- Edge cases: Invalid sample, user stops early, device disconnect, blocked or unstable airflow, no baseline yet, baseline reset after a significant product/account event if defined later.
- Acceptance criteria: Users can complete oral setup, see baseline progress, complete a valid oral session, and view a result that clearly shows score, baseline status, and trend history without ambiguity.
- Success metrics: Baseline completion rate, first oral session completion rate, 7-day repeat oral use, oral session recovery rate after disconnect.

### Fat Burning

- Objective: Help users capture repeated fat-burning readings in a single session and understand whether they are moving toward a target.
- User value: A guided repeated-reading flow that shows actionable progress within one session and across completed sessions.
- Design scope: Setup, repeated-reading coach, target-based progress, session summary, history, and recovery handling for fat sessions.
- Relevant PRD requirements: Acetone and CO2 sensors, session-relative delta, positive delta is desirable, best delta is the highest valid in-session delta.
- Hardware touchpoints: Handheld enclosure, device power button, BLE state, sensor readiness, session completion signal, airflow-conditioning path.
- Software touchpoints: Mode selection, repeated measurement coach, current/best delta display, goal target, session summary.
- Interaction spec: The app must separate current delta from best delta so the user can understand live movement versus the strongest session performance. The finish action is only available after at least one valid reading, and the physical product should make repeat measurements feel stable without exposing sensors directly to breath.
- State and behavior spec: Show `0%` at the first valid reading, then update current delta and best delta independently. The final summary should declare goal achieved only when best delta meets or exceeds target.
- Edge cases: No target set, negative readings, user finishes early, device disconnect, sensor failure, repeated invalid samples, unstable airflow.
- Acceptance criteria: Users can complete the fat flow, see repeated readings, understand that higher delta is better, and view a final summary with current delta, best delta, goal target, and goal achieved status.
- Success metrics: First fat session completion rate, average valid readings per session, target attainment rate, fat session repeat rate within 30 days, recovery from interrupted reading.

### Trial, entitlement, and read-only access

- Objective: Make access state understandable at every app entry point.
- User value: Users know immediately whether they can measure now or only review history.
- Design scope: Trial banner, expired state, temporary access, read-only mode, reactivation prompts.
- Relevant PRD requirements: 60-day trial, post-trial subscription, cached entitlement freshness, history visibility in read-only mode.
- Hardware touchpoints: None beyond preventing new session starts when state blocks measurement.
- Software touchpoints: Home, settings, paywall, session entry controls, history visibility.
- Interaction spec: Use explicit labels `Temporary access` and `Read-only mode` instead of vague status language. Block start-session actions while preserving history access.
- State and behavior spec: Fresh cached access can still let the user browse history, but never start a new session without recheck or valid entitlement.
- Edge cases: Backend unavailable, offline start, stale cache, expired subscription, restore purchase failure.
- Acceptance criteria: Users can always tell why a session is blocked, what they can still do, and what action restores measurement access.
- Success metrics: Entitlement-screen comprehension, blocked-session help click-through, restore-purchase success rate, support contact reduction for access confusion.

### Consult professionals

- Objective: Provide a clearly bounded support handoff that helps users find external educational or professional resources without implying diagnosis or hidden data sharing.
- User value: Users can get next-step support from the context of a feature without losing trust in what AirHealth is and is not doing.
- Design scope: Feature-card entry, result/history entry, support directory, external-handoff notice, unavailable-resource fallback.
- Relevant PRD requirements: Phase 1 informational directory only, both features supported, no account/result/breath-data transmission, available even in temporary access and read-only states.
- Hardware touchpoints: None, aside from action disabling when an active measurement or wake transition is in progress.
- Software touchpoints: Home task hub, result summary, history detail, locale filtering, external destination handoff.
- Interaction spec: The support directory must look different from measurement and suggestion content, and every exit to an external destination must pass through a short disclosure state first.
- State and behavior spec: Available in non-measurement contexts for both features; disabled while another action is active; if no localized content exists, the screen falls back to generic educational support rather than going empty.
- Edge cases: No localized resource, dead external link, external phone app unavailable, user in read-only mode, user returns from an external resource.
- Acceptance criteria: Users can open the directory from both features, understand that no health data is shared, and complete an external handoff without mistaking it for in-app booking or medical advice.
- Success metrics: Directory open rate, external-handoff completion rate, dead-link recovery rate, and support-flow comprehension.

### Low-power readiness and wake

- Objective: Communicate idle power-saving behavior without making the device appear broken or disconnected.
- User value: The device feels battery-conscious but still responsive and trustworthy.
- Design scope: Home/dashboard ready states, wake messaging, measurement start from low power, low-power-ready banner, and hysteresis-safe UI transitions.
- Relevant PRD requirements: Low-power entry after 3 consecutive seconds below 1% change, exit after user/app action or 2 consecutive seconds above 2% change, no interruption of active measurement transitions.
- Hardware touchpoints: Firmware low-power state, wake trigger, readiness indicator.
- Software touchpoints: Home status banner, feature-hub action gating, measurement start CTA, reconnect/help messaging.
- Interaction spec: Low power must be presented as a normal idle-ready state, not an error. Waking the device should feel like a short readiness step rather than a reconnect flow unless BLE is actually lost.
- State and behavior spec: The UI shows `Low-power ready` when idle, prevents measurement-start confusion during wake, and avoids flicker by matching the hysteresis band used by firmware.
- Edge cases: Threshold-hovering noise, wake while another action is selected, stale entitlement during wake, wake timeout.
- Acceptance criteria: Users can tell the difference between low power and failure, can wake the device without leaving the feature context, and do not experience visible state chatter near the threshold.
- Success metrics: Low-power wake success rate, false-failure perception rate, wake-to-action time, and support-contact reduction for idle-state confusion.

### Sharing to Apple Health and Health Connect

- Objective: Export completed session summaries to platform health ecosystems without exposing raw or private data.
- User value: Preserve useful trend continuity in systems the user already trusts.
- Design scope: Export permission flow, completion confirmation, failure states, and field disclosure.
- Relevant PRD requirements: Phase 1 sharing only, completed summaries only, no raw sensor streams, no account identifiers, no purchase recommendations.
- Hardware touchpoints: None beyond completed session generation on the device.
- Software touchpoints: Result screen, sharing settings, platform permission prompts.
- Interaction spec: The sharing UI should explain what will be shared, what will not, and which destination the export will use.
- State and behavior spec: If a destination does not support a field, omit it rather than mapping to a substitute metric.
- Edge cases: Permission denied, unsupported destination, destination schema mismatch, export retry after offline completion.
- Acceptance criteria: The user can share a completed session to the platform destination on the supported OS, and only approved summary fields are exported.
- Success metrics: Export opt-in rate, export success rate, permission-denial recovery rate, share-after-completion rate.

## 9. Acceptance Criteria

- Every major flow has at least one explicit recovery path and one explicit failure state.
- The app never shows a final measurement result before device confirmation.
- The app never allows two active sessions at the same time.
- The app clearly distinguishes Oral & Dental Health and Fat Burning result semantics.
- The app labels access states as `Temporary access` and `Read-only mode` when entitlement verification is unavailable or stale.
- The user can always see whether history is available, whether new sessions are blocked, and what action can restore access.
- The feature-card task hub always shows one consistent action vocabulary, with blocked actions explained rather than hidden.
- The `consult professionals` flow is clearly labeled as an external support directory and never implies in-app booking or data sharing.
- Low-power-ready is visually distinct from disconnected or failed states, and wake transitions do not flicker or strand the user.
- Completed session sharing excludes raw sensor data, purchase recommendations, and account identifiers.
- Factory mode is only available through authorized internal tooling before consumer provisioning, and the consumer app never exposes Factory entry, Factory logs, or Factory lockout controls.
- The 3-color LED behavior is documented clearly enough that internal factory users can interpret orange, green, and red states without relying on consumer copy.
- HW-ID remains internal-only and never appears in consumer screens, settings, share flows, or exported summaries.
- The design uses the same core layout and status grammar across all states so Figma variants can be built efficiently.

## 10. Success Metrics

- Pairing completion rate within the first session.
- First measurement completion rate within 24 hours of pairing.
- Oral baseline completion rate within the first 5 valid oral sessions.
- Fat session completion rate with at least 1 valid reading.
- Repeat session rate within 7 and 30 days.
- Error recovery rate for pairing, disconnect, and export failures.
- First-try action selection rate from the feature hub.
- Consult-professionals handoff completion rate.
- Entitlement comprehension rate, measured by users correctly identifying whether they can measure now.
- Low-power wake success rate within the first attempt.
- Sharing success rate on supported platforms.
- Factory pass/fail capture rate before shipment.
- BLE log capture success rate during Factory mode.
- HW-ID routing coverage across manufacturing, backend, analytics, and support workflows.
- Support-case resolution time by hardware profile.
- Support contact rate for subscription and blocked-access confusion.

## 11. Figma Implementation Guidance

### Frames or pages needed

- Onboarding and permissions
- Pairing
- Home and mode selection
- Feature-card action hub
- Oral measurement
- Fat measurement
- Consult professionals directory
- Results and history
- Subscription and entitlement
- Sharing settings
- Error and recovery states
- Device industrial design exploration
- Handheld airflow-path variants
- Factory verification
- Internal HW-ID routing and support

### States that require separate screens or variants

- Unpaired, paired, ready, disconnected
- Feature hub ready, feature action locked, low-power-ready, waking
- Measuring oral, measuring fat, complete, canceled, failed
- Trial active, paid active, temporary access, read-only mode
- Permission granted, permission denied
- Export success, export failure
- External support handoff notice and external-resource-unavailable
- Factory mode in progress, Factory pass, Factory fail, Factory locked
- HW-ID mapping missing, HW-ID lookup available

### Components likely needed

- Status banner
- Mode card
- Action chip row
- Score card
- Step coach
- Progress bar
- Trend sparkline
- Error callout
- Low-power-ready banner
- External-handoff notice
- Primary CTA button
- Secondary CTA button
- Permission modal
- Share sheet summary panel
- Device shell views
- Mouth-contact / airflow-path callouts
- Indicator light states
- Factory status banner
- LED legend
- BLE log capture panel
- HW-ID routing chip or badge
- Cross-section / cutaway annotations

### Reusable patterns

- Single-column mobile layout
- Top status area with connection and entitlement state
- Bottom sticky primary action
- Result summary card with supporting trend row
- Short recovery message pattern with retry action

### Annotations required

- State dependencies
- Disabled vs hidden actions
- What data is stored or shared
- Which states are read-only
- Which copy is platform specific
- Which parts of the breath path are fixed, passive, or user-replaceable
- Which screens are consumer-facing versus internal-only
- Where Factory logs and HW-ID values may appear in authorized tooling

### Prototyping needs

- Pairing to ready transition
- Feature hub to action lock transition
- Oral measurement to complete transition
- Fat measurement repeated-reading loop
- Low-power ready to wake to measure transition
- Consult professionals directory to external handoff
- Expired entitlement to read-only mode transition
- Share permission grant and failure paths
- Factory long-press entry, check, pass/fail, and lockout transitions
- Internal HW-ID lookup and factory log review

### Open questions before final Figma production

- Final visual system and brand treatment
- Final copy for trial expiration and payment recovery
- Exact legal text for health sharing disclosures
- Final taxonomy and sourcing rules for the curated professional-support directory
- Final copy for low-power-ready and wake messaging
- Whether any device indicator beyond power state will ship in Phase 1
- Final owner and schema for HW-ID routing tables and factory log storage
- Whether the internal factory console is a standalone tool or embedded support surface

## 12. Open Questions and Design Risks

- The product still depends on sensor accuracy and warm-up behavior that may require visual tuning once hardware prototypes exist.
- Final legal and privacy review may require copy changes for health sharing, subscription, and wellness claims.
- The lack of a device display means the mobile app must carry all instructional clarity; this is a usability risk if content becomes too dense.
- Cached entitlement messaging must stay consistent across app surfaces to avoid support confusion.
- The new feature-card action hub increases Home-screen complexity and will need careful visual prioritization to avoid choice overload.
- The support-directory experience depends on content operations and external links staying fresh enough to remain trustworthy.
- Low-power-ready messaging must stay distinct from disconnect or failure states, or users may misread a healthy idle device as broken.
- Factory mode exposure must be blocked consistently across provisioning, support, and shipped-device states, or a consumer could see internal-only controls.
- HW-ID taxonomy drift could fragment manufacturing, analytics, and support records if the routing table is not owned and versioned carefully.
- If the LED and app disagree during Factory mode, internal operators may misread the unit's status unless the log result is visually prioritized.
- If future firmware introduces additional device indicators, the design system will need a companion state spec update.
