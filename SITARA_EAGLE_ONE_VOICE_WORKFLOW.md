# SITARA — Eagle One Voice Workflow (Non-US)

**Version:** 1.0.1  
**Date:** October 14, 2026  
**Status:** Proposed interaction specification  
**Owner:** TactixGlobal

**Market:** Non-US  
**Default spoken/display units:** Meters  
**US counterpart:** [STELLA](STELLA_EAGLE_ONE_VOICE_WORKFLOW_US.md)

## Version history

| Version | Date | Changes |
|---|---|---|
| 1.0.1 | 2026-10-14 | Designated SITARA as the non-US edition; linked STELLA US edition. Existing interaction behavior preserved. |
| 1.0.0 | 2026-10-14 | Initial voice workflow for tethered Eagle One: deployment, camera interaction, clarification, evidence sharing, exceptions, and recovery. |

Update this document in place so its shareable link remains stable. Record each revision above and in a Git commit. Use major versions for incompatible interaction changes, minor versions for added workflows, and patch versions for clarifications.

## Operating model

SITARA is the voice interface. Eagle One is the tethered observation drone. Startup checks have passed. Video, recording, and routine monitoring run automatically.

This example assumes an authorized Fotokite integration exposes the required controls, the north entrance is mapped, and deployment heights are relative to the ground station. It describes proposed behavior, not verified Fotokite API capabilities.

## 1. Request deployment

**Operator:** “Hey Sitara, deploy Eagle One to 30 meters and watch the north entrance.”

SITARA silently checks current conditions and whether the request is supported.

**Sitara:** “Deploy Eagle One to 30 meters, watching the north entrance. Confirm launch?”

**Operator:** “Confirm.”

If something blocks deployment, explain the specific issue instead of asking for confirmation.

## 2. Automatic deployment

The workflow:
- Launches Eagle One.
- Opens live video.
- Starts configured recording.
- Points the camera toward the mapped entrance.

After telemetry confirms the requested deployment:

**Sitara:** “Eagle One on station.”

No repetitive “mission ready” or battery announcements. Height, power health, recording, and camera mode remain visible on screen.

## 3. Change the view

Several minutes later:

**Operator:** “Hey Sitara, switch Eagle One to thermal.”

Thermal appears with an on-screen label and a short acknowledgement tone.

During the active follow-up window:

**Operator:** “Zoom in on the gate.”

**Sitara:** “Vehicle gate or pedestrian gate?”

**Operator:** “Pedestrian gate.”

The camera centers and zooms. No unnecessary spoken narration.

## 4. Interrupt or correct

**Operator:** “No, slightly left.”

SITARA stops speaking if necessary and applies the correction.

Corrections made before command execution replace the earlier request. If an action has already started, SITARA reports its actual state and cancels only where supported.

## 5. End listening automatically

After inactivity, the listening indicator turns off.

Observation and recording continue. The operator does not need to repeat “keep watching.”

A system announcement does not automatically reopen command listening.

## 6. Share evidence

**Operator:** “Hey Sitara, send Eagle One’s last fifteen seconds to command.”

Transfer progress appears on screen.

After receipt is acknowledged:

**Sitara:** “Clip delivered to command.”

If disconnected:

**Sitara:** “Command link unavailable. Clip saved and queued.”

Retries follow the configured policy without repeated announcements.

## 7. Announce actionable exceptions

Normal tether power: stay quiet.

If telemetry confirms external power failure and automatic landing:

**Sitara:** “Eagle One: external power lost. Automatic landing initiated.”

If only video fails:

**Sitara:** “Eagle One video unavailable.”

Never confuse video loss with flight-control loss or claim an action that telemetry has not confirmed. Report backup-power status when relevant; do not invent remaining flight time from battery percentage.

## 8. Land

**Operator:** “Hey Sitara, land Eagle One.”

The system checks whether recovery can proceed. If landing-area clearance is required:

**Sitara:** “Confirm the landing area is clear.”

**Operator:** “Clear.”

Once required clearance is resolved, issue the landing command. After the platform confirms recovery has started:

**Sitara:** “Eagle One landing.”

After verified landing and disarming:

**Sitara:** “Eagle One landed.”

Recording closes, evidence is preserved, and the mission log is saved automatically.

## Regional consistency

SITARA and STELLA are regional identities for the same intended interaction model. Review shared behavior changes across both editions. Preserve explicit unit requests and clarify ambiguous units; heights in this example remain relative to the ground station.

## Developer rules

- Wake phrase: **“Hey Sitara.”** Product spelling: **SITARA**; intended pronunciation: **sih-TAH-rah**. Validate accent variants.
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

**Design principle:** Request the task once, adjust it naturally, and hear from SITARA when something meaningful changes.
