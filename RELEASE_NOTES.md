# SpaNET fork v1.2.0-rc.1 — release candidate

This is a pre-release of the community SpaNET integration based on
[lloydw/hass-spanet v1.1.0](https://github.com/lloydw/hass-spanet/releases/tag/v1.1.0).
It is intended for controlled testing, not broad production rollout.

## Added
- Actual pump-speed capability detection, without assuming all pumps have one speed.
- Multi-speed pump mode state sensor.
- Four-option **Off / Auto / Low / High** selector on controllable **exactly two-speed** pumps.
- Cloud-confirmed readback after a mode command (first request after about 7 seconds, one bounded retry after a further 15 seconds).
- More resilient startup when the SpaNET API temporarily returns HTTP 202: avoid incomplete entity setup and let Home Assistant retry.
- Offline tests, Home Assistant container import and hassfest CI.

## Preserved
- Existing single-speed Pump On/Off switch and legacy command payload.
- Original switch entity IDs; the new selector is additive for supported hardware.
- Standard 300-second background pump polling; rapid readback is only after Home Assistant mode commands.

## Verified
- Tested on a Pembroke Spa with SmartLINK V2 and a physical two-speed Pump 1.
- Off, Auto, Low and High commands confirmed using the official SpaNET app and physical pump response.
- Home Assistant's mode sensor updated in roughly 7 seconds for successful High and Off commands.
- 38 offline regression tests, Home Assistant import check, and hassfest passed on the feature-complete Stage 4 commit.

## Known limitations and warnings
- **Pre-release:** The experimental two-speed selector is currently enabled in the branch. Install only if you understand the effects of spa pump commands and are able to supervise initial testing.
- Single-speed compatibility has offline test coverage, but has not yet been verified with physical single-speed spa hardware.
- Startup HTTP 202 automatic-retry logic has offline test coverage, but has not been deliberately tested against a live outage.
- Changes made using the official SpaNET app continue to depend on normal background polling; the faster readback applies to commands issued through the new HA selector.
- The custom SpaNET dashboard card is a separate project and is not included here.

## Updating and rollback
Back up the existing `custom_components/spanet` folder before replacing it.
Keep a known-good version available for rollback. On a failed startup, inspect
Home Assistant logs and try reloading the integration before rolling back.

## Attribution
Based on [lloydw/hass-spanet](https://github.com/lloydw/hass-spanet),
maintained by Lloyd Weehuizen and other contributors, under its existing MIT
license. This is a fork test release and is **not** an upstream release.
