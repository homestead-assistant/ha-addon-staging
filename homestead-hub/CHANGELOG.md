# Changelog

## 0.3.0

**Breaking — this is a reinstall, not an upgrade.** The add-on slug changed from
`homestead_hub` to `homestead-hub`. Home Assistant identifies an add-on by its
slug, so the old add-on will show as unavailable and this one appears as a
separate, uninstalled add-on. Add-on configuration and data live under the slug
and do **not** carry over: note your options, uninstall the old add-on, install
this one, and re-enter them. Back up `/addon_configs` first if you want a copy
of the old data.

- Adds the **storefront API**: external shops pull plant availability from the
  Hub and claim the stock they intend to sell. Claimed plants stay in the Hub
  and keep being cared for, but stop counting as available to other storefronts.
  Keys are issued on the host with `storefront-key`; see `docs/STOREFRONT_API.md`.
- Removes the previous single-storefront inventory push and its Inventory page.
  The Hub no longer calls out to any storefront and holds no storefront
  credentials. This changes no add-on option — the retired settings were
  environment variables on the standalone Docker lane, never exposed here.

## 0.1.2

- Detects Home Assistant add-on mode so onboarding, integrations, remote
  access, and system status copy use Home Assistant-native guidance.
- Adds Supervisor-aware service status rendering when Supervisor API access is
  available.
- Records binary Home Assistant entity states such as `on`, `off`, `open`,
  `closed`, `wet`, and `dry` as `1`/`0` readings for automations.
- Shows whether MQTT was discovered through Supervisor, manually configured,
  or unavailable.
- Adds translated add-on option labels and descriptions for the Home Assistant
  configuration UI.
- Updates the add-on README for onboarding, ZHA, MQTT, remote access, and
  direct-port caveats.

## 0.1.1

- Adds support for the in-app first-run setup flow when `admin_password` is
  left blank.
- Treats MQTT as recommended rather than mandatory so ZHA-only Home Assistant
  setups can start without a broker.
- Improves startup logs for admin bootstrap, MQTT discovery, and HA entity
  polling.

## 0.1.0

- Initial release as a Home Assistant add-on.
- Wraps `ghcr.io/chickenassistant/core` with a Supervisor-aware entrypoint
  that translates user options into env vars and persists state under `/data`.
- Requires an MQTT broker (`services: [mqtt:need]`). Credentials are
  discovered automatically when the HA Mosquitto add-on is installed;
  individual `mqtt_*` options override discovery.
- Requests `homeassistant_api: true` so the add-on can poll HA entity
  states directly (ZHA integration).
- New `ha_entities` option maps HA entities (ZHA sensors, template
  sensors, etc.) onto Chicken Assistant sensor readings; `ha_poll_interval`
  controls the cadence. The polling pipeline reuses the same persistence
  layer as the MQTT ingestion path.
- Auto-generates `SESSION_SECRET` on first run when unset.
