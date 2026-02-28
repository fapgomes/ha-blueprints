# Changelog

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
