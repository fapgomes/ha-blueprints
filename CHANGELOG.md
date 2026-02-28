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
