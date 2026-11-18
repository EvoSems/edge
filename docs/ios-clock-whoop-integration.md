# iPhone Clock → WHOOP vibration integration

This branch builds on the **existing** `BuzzBandIntent` in `ios/OpenStrapIntents.swift` and `ShortcutSyncBridge.shared.buzz()`. Do not duplicate the vibration protocol.

## First working integration: Apple Clock alarms

In iOS Shortcuts:
1. Automation → + → Alarm.
2. Select **Goes Off** → **Any**.
3. Select **Run Immediately** (if offered).
4. Add action **Buzz Band** from OpenStrap.
5. Test while OpenStrap is already connected to WHOOP.

This reuses existing Clock alarms, including newly created alarms. It does not mirror all timer completions, read the Clock alarm database, or guarantee background BLE execution.

## Planned improvements

- Expose a user-friendly test screen for the existing Buzz Band action, reporting connected / disconnected / command result rather than silently claiming success.
- Add a setting to enable a short repeat vibration pattern, with a safe upper limit and a stop action, after confirming protocol semantics.
- Improve diagnostics for Shortcut invocation while the Flutter app is suspended.
- Offer app-owned timers (AlarmKit on supported iOS versions) with an explicit fallback when background BLE delivery is unavailable.
- Add a live heart-rate presentation using the existing app stream; any Dynamic Island Live Activity must respect ActivityKit background update limits.

## Constraints

- Apple's public APIs do not let an ordinary app enumerate all existing Clock alarms or intercept every Clock timer completion.
- Avoid private APIs, undocumented Clock database access, and unsupported promises of guaranteed wake-up vibration.
- Preserve existing WHOOP 4.0 behavior, particularly firmware 41.17.6.0 / Bluetooth 17.2.2.0.
- Test real device background execution, app terminated, disconnected band, Bluetooth off, and multiple alarms.
- Keep changes isolated to this feature branch until validated.

## Source inspected

- `ios/OpenStrapIntents.swift` already declares `BuzzBandIntent` and registers it in `OpenStrapShortcuts`.
- `AGENTS.md` documents separation between protocol and edge UI/orchestration.

This file is an implementation plan, **not a claim that the new timer and live-HR features have been implemented**.
