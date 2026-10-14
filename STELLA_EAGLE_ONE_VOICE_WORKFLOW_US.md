# STELLA — Eagle One Voice Workflow (US)

**Version:** 1.0.0  
**Date:** October 14, 2026  
**Status:** Proposed interaction specification  
**Owner:** TactixGlobal

**Market:** United States  
**Default spoken/display units:** Feet  
**Non-US counterpart:** [SITARA](SITARA_EAGLE_ONE_VOICE_WORKFLOW.md)

## Version history

| Version | Date | Changes |
|---|---|---|
| 1.0.0 | 2026-10-14 | Initial US edition derived from SITARA v1.0.0: STELLA voice identity and 100-foot deployment example. |

Update this document in place so its shareable link remains stable. Record each revision above and in a Git commit. Use major versions for incompatible interaction changes, minor versions for added workflows, and patch versions for clarifications.

## Operating model

STELLA is the voice interface. Eagle One is the tethered observation drone. Startup checks have passed. Video, recording, and routine monitoring run automatically.

This example assumes an authorized Fotokite integration exposes the required controls, the north entrance is mapped, and deployment heights are relative to the ground station. It describes proposed behavior, not verified Fotokite API capabilities.

## 1. Request deployment

**Operator:** “Hey Stella, deploy Eagle One to 100 feet and watch the north entrance.”

STELLA silently checks current conditions and whether the request is supported.

**Stella:** “Deploy Eagle One to 100 feet, watching the north entrance. Confirm launch?”

**Operator:** “Confirm.”

If something blocks deployment, explain the specific issue instead of asking for confirmation.

## 2. Automatic deployment

The workflow:
- Launches Eagle One.
- Opens live video.
- Starts configured recording.
- Points the camera toward the mapped entrance.

After telemetry confirms the requested deployment:

**Stella:** “Eagle One on station.”

No repetitive “mission ready” or battery announcements. Height, power health, recording, and camera mode remain visible on screen.

## 3. Change the view

Several minutes later:

**Operator:** “Hey Stella, switch Eagle One to thermal.”

Thermal appears with an on-screen label and a short acknowledgement tone.

During the active follow-up window:

**Operator:** “Zoom in on the gate.”

**Stella:** “Vehicle gate or pedestrian gate?”

**Operator:** “Pedestrian gate.”

The camera centers and zooms. No unnecessary spoken narration.

## 4. Interrupt or correct

**Operator:** “No, slightly left.”

STELLA stops speaking if necessary and applies the correction.

Corrections made before command execution replace the earlier request. If an action has already started, STELLA reports its actual state and cancels only where supported.

## 5. End listening automatically

After inactivity, the listening indicator turns off.

Observation and recording continue. The operator does not need to repeat “keep watching.”

A system announcement does not automatically reopen command listening.

## 6. Share evidence

**Operator:** “Hey Stella, send Eagle One’s last fifteen seconds to command.”

Transfer progress appears on screen.

After receipt is acknowledged:

**Stella:** “Clip delivered to command.”

If disconnected:

**Stella:** “Command link unavailable. Clip saved and queued.”

Retries follow the configured policy without repeated announcements.

## 7. Announce actionable exceptions

Normal tether power: stay quiet.

If telemetry confirms external power failure and automatic landing:

**Stella:** “Eagle One: external power lost. Automatic landing initiated.”

If only video fails:

**Stella:** “Eagle One video unavailable.”

Never confuse video loss with flight-control loss or claim an action that telemetry has not confirmed. Report backup-power status when relevant; do not invent remaining flight time from battery percentage.

## 8. Land

**Operator:** “Hey Stella, land Eagle One.”

The system checks whether recovery can proceed. If landing-area clearance is required:

**Stella:** “Confirm the landing area is clear.”

**Operator:** “Clear.”

Once required clearance is resolved, issue the landing command. After the platform confirms recovery has started:

**Stella:** “Eagle One landing.”

After verified landing and disarming:

**Stella:** “Eagle One landed.”

Recording closes, evidence is preserved, and the mission log is saved automatically.

## US localization

- Default spoken and displayed height to feet; preserve explicit metric requests and confirm units when ambiguous.
- Convert units accurately at the integration boundary and enforce manufacturer limits after conversion. The 100-foot example is 30.48 meters, not exactly 30 meters.
- Height remains relative to the ground station; do not silently reinterpret it as height above terrain.
- STELLA and SITARA are regional identities for the same intended interaction model. Changes to shared behavior should be reviewed across both editions.

## Developer rules

- Wake phrase: **“Hey Stella.”** Product spelling: **STELLA**; intended pronunciation: **STELL-uh**. Validate accent variants.
- Dedicated push-to-talk supports the same workflow without a wake phrase.
- Allow brief follow-ups without repeating the wake phrase.
- Clearly display listening state and selected asset.
- Require an explicit asset name when switching targets.
- Ask when location, altitude, asset, or camera target is unclear.
- Launch confirmation applies only to the exact pending request and expires.
- Expire unresolved requests; never execute stale commands later.
- Use visual feedback for routine changes.
- Speak for meaningful completion, actionable exceptions, or clarification.
- Distinguish requested, acknowledged, executing, and completed states.
- Keep manufacturer protections, recovery procedures, and manual controls independent of voice.
- Execute only controls supported by the authorized integration.

**Design principle:** Request the task once, adjust it naturally, and hear from STELLA when something meaningful changes.
