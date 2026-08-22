# Changelog

## 2026-08-22

### All blueprints

- Replace the `not_from: [unavailable, unknown]` trigger filter with a state
  freshness condition (reject events whose state timestamp is older than 5s).
  `not_from` also swallowed the first genuine press after a Home Assistant
  restart, because restored event entities start at `unknown`
  (home-assistant/core#142263). The freshness test blocks the same stale
  replays without that blind spot, and being mechanism-independent it also
  covers retained-MQTT replays. `not_to` is kept — it suppresses the harmless
  outbound half of the availability flap. Introduced in
  zigbee2mqtt-tuya_1_to_4_buttons by @GabrielGoldsteinAnidea (#2) and extended
  to the remaining blueprints (zigbee2mqtt-nedis-remote,
  zigbee2mqtt-ikea_tradfri, zigbee2mqtt-aqara-cube,
  zigbee2mqtt-green_power_button).

### zigbee2mqtt-tuya_1_to_4_buttons.yaml

- Fix unprefixed-action dispatch: the button 1 branches matched a bare
  `single`/`double`/`hold` unconditionally, so on devices that emit an
  unprefixed action plus a `button` attribute, holds (and presses) of
  buttons 2-4 ran button 1's action. An unprefixed action now routes by the
  `button` attribute; only a device that sends no `button` attribute at all
  (single-button remotes) still maps to button 1, and an unparseable `button`
  value (explicit null, `both`, ...) falls through to the logbook default
  instead of being misrouted. Supersedes the unconditional bare-`hold`
  handling introduced on 2026-02-28. Hold dispatch fixed by
  @GabrielGoldsteinAnidea (#3) and generalized to `single`/`double`.

### zigbee2mqtt-aqara-cube.yaml

- Prefer the event's own `side` attribute over the derived `*_side` sensor for
  side detection. Reading the sensor at trigger time could race the update
  carried by the same MQTT message (running the previous side's action) and
  silently broke all per-side actions if either entity was renamed. Events
  without a `side` attribute (e.g. rotation) still fall back to the sensor.

### zigbee2mqtt-green_power_button.yaml

- Let any non-empty `event_type` pass the automation condition so the
  `default` logbook branch becomes reachable, serving as a discovery aid for
  unmapped events like in the other blueprints (it was dead code: the
  condition only admitted the three event types the `choose` already handled).

## 2026-07-16

### All blueprints

- Add `not_from: [unavailable, unknown]` trigger filter to all blueprints
  (zigbee2mqtt-nedis-remote, zigbee2mqtt-tuya_1_to_4_buttons,
  zigbee2mqtt-ikea_tradfri, zigbee2mqtt-aqara-cube,
  zigbee2mqtt-green_power_button). When zigbee2mqtt or the MQTT broker
  restarts, the `event.*_action` entities transition from `unavailable` back
  to their last (old) event timestamp, and that state-restore transition was
  firing the automations as if the buttons had been pressed — opening gates,
  doors, and moving covers. `not_to` alone does not cover this case because
  the restored "to" state is a valid timestamp.

## 2026-02-28

### zigbee2mqtt-tuya_1_to_4_buttons.yaml

- Fix hold actions not working due to string vs integer comparison on button attribute
- Add support for `N_hold` event type patterns (e.g., `1_hold`, `2_hold`) alongside `hold` + button attribute
- Add consistent unprefixed `hold` handling for button 1 (matching single/double behavior)
- Replace deprecated `data_template` with `data`
- Add `not_to: [unavailable, unknown]` trigger filter to avoid unnecessary fires
- Add `default('')` guard on `event_type` condition to prevent errors
- Improve blueprint description and add `source_url` for community sharing
- Fix `source_url` from `fapg` to `fapgomes`

### zigbee2mqtt-nedis-remote.yaml

- Improve blueprint description and add `source_url`
- Add `not_to: [unavailable, unknown]` trigger filter
- Add `default('')` guard on `event_type` condition
- Add logbook fallback for unknown events

### zigbee2mqtt-ikea_tradfri.yaml

- Improve blueprint description and add `source_url` (model E1812)
- Fix input names and descriptions to be user-friendly (e.g., `"on action"` → `"Single Press"`)
- Fix typo `"occour"` and incorrect action descriptions
- Add `not_to: [unavailable, unknown]` trigger filter
- Add `default('')` guard on `event_type` condition
- Add logbook fallback for unknown events

### zigbee2mqtt-aqara-cube.yaml

- Improve blueprint description and add `source_url`
- Replace deprecated `service:` with `action:` in logbook default
- Remove redundant nested `- sequence:` inside choose sequences
- Add `not_to: [unavailable, unknown]` trigger filter
- Fix side sensor name construction (was resolving to `_action_side` instead of `_side`)
- Add support for all 10 event types: tap, shake, slide, flip180, flip90, throw, wakeup, fall, rotate_left, rotate_right
- Change `side` default from `0` to `-1` to avoid false matches when sensor is unavailable

### zigbee2mqtt-green_power_button.yaml

- Improve blueprint description and add `source_url`
- Move `variables` block after `trigger` for correct ordering
- Fix input names to be user-friendly (e.g., `"off action"` → `"Button 1 (Off)"`)
- Add `default('')` guard on `event_type` variable
- Add `not_to: [unavailable, unknown]` trigger filter
- Add logbook fallback for unknown events
