# Home Assistant IntesisHome integration (with config flow)
Development fork of the IntesisHome integration for Home Assistant

*This project has been forked from https://github.com/jnimmo/hass-intesishome for my personal use and has been modified to suit my own needs. Use at your own risk.*

[![hacs_badge](https://img.shields.io/badge/HACS-Custom-41BDF5.svg?style=for-the-badge)](https://github.com/hacs/integration)

This custom integration is a fork of the core integration for IntesisHome. It retains cloud control support for IntesisHome, anywAir, and airconwithme devices, and adds fixes for local HTTP control of MH-AC-WIFI-1 devices.

## Fixes in this fork

- **Vendored pyintesishome library** — uses relative imports to load the bundled library instead of any system-installed version, preventing conflicts with the built-in HA intesishome component
- **AUTO fan mode for MH-AC-WIFI-1** — these devices support AUTO fan speed (UID 4 value 0) even though it is not advertised in the local datapoints descriptor; this fork injects AUTO into the fan mode list for MH-AC-WIFI-1 devices
- **None guard in get_mode_list** — prevents a TypeError crash when cloud devices do not return UID 61 (config_mode_map)
- **KeyError guard on config entry reload** — prevents a crash when reloading the integration via the UI
- **HA 2026.x config flow compatibility** — replaces deprecated `FlowResult` import with `ConfigFlowResult` to fix "Invalid handler specified" error when adding the integration via the UI
- **Remove deprecated async_step_import** — removes the deprecated YAML import handler that causes "Invalid handler specified" error when adding the integration via the UI in HA 2025+
- **Replace deprecated async_forward_entry_unload** — replaces removed `async_forward_entry_unload` with `async_unload_platforms` for HA 2025+ compatibility
- **Relative imports in vendored library** — fixes absolute imports within pyintesishome submodules that caused `No module named 'pyintesishome.intesisbase'` error on production

## Tested hardware

| Device       | Mode              | Status             | Notes |
| ------------ | ----------------- | ------------------ | ----- |
| MH-AC-WIFI-1 | intesishome_local | :white_check_mark: | Tested on 3 units, firmware 1.3.1. AUTO fan mode working. |

Other devices supported by the upstream integration may work but are untested in this fork.

## Configuration
1. Add this custom repository to HACS, or manually download the files into your custom_components directory
2. Restart Home Assistant
3. Navigate to Settings → Devices & Services → Add Integration and search for "IntesisHome"
4. Select the device type
5. Provide any additional required details (username, password, IP address) for your device

## Cloud control
Control of IntesisHome, anywAir, airconwithme devices is through a persistent connection to the Intesis cloud. This requires outgoing HTTPS access to connect to the API, then control moves to a TCP port specified by the API.

No changes have been made to the cloud control path from the upstream fork. Cloud functionality is untested in this fork but should work as per the original.

## Local control

### HTTP (intesishome_local)
Allows local control over HTTP for devices which expose an HTTP web server on port 80 (http://ip/api.cgi)

| Device        | HTTP - intesishome_local |
| ------------- |:------------------------ |
| MH-AC-WIFI-1  | :white_check_mark:       |
| DK-RC-WIFI-1B | :white_check_mark:       |
| FJ-AC-WIFI-1B | :white_check_mark:       |
| IS-ASX-WIFI-1 | :x:                      |

Note: DK-RC-WIFI-1B and FJ-AC-WIFI-1B are listed as supported by the upstream fork but are untested in this fork.

## Acknowledgements

This integration includes a vendored copy of [pyIntesisHome](https://github.com/jnimmo/pyIntesisHome) by James Nimmo, licensed under MIT. The library has been modified — see the git log for details.