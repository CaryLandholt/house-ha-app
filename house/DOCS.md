# House

The house at a glance, as a Home Assistant sidebar panel: battery health,
room occupancy, internet, climate, security, cameras, power and systems, with
two controls - a room's lights and talking through a camera's speaker.

Everything it shows comes from Home Assistant, through the Supervisor's proxy
with the token the Supervisor injects; no long-lived token is needed. The only
other credential is optional: a UniFi Protect API key for LIVE talk-back.

## Installing

The image is **private** on GHCR (it inherits this repository's visibility),
so the Supervisor needs credentials to pull it. Once, before installing: in
Settings → Apps → App store, open the ⋮ menu → **Registries**, and add
`ghcr.io` with your GitHub username and a personal access token that has
`read:packages`. Then add `https://github.com/CaryLandholt/house-ha-app` under
**Repositories** and install House. That public repository holds the App
metadata only — the Supervisor fetches App repositories with a plain `git
clone`, which the private source repository refuses — and is synced from
`ha-app/` here on every version bump.

## Options

| option | what it is |
| --- | --- |
| `unifi_protect_url`, `unifi_protect_api_key` | UniFi Protect and an API key, for LIVE talk-back; leave empty and talk-back plays a clip through Home Assistant instead |
| `ha_web_base` | where a browser reaches Home Assistant, for the "open in HA" links - `https://ha.example` |
| `ha_mobile_base` | the companion app's scheme, `homeassistant://` |

The key can come from Home Assistant's own `secrets.yaml`: in the
Configuration tab, switch to YAML and write
`unifi_protect_api_key: !secret unifi_protect_api_key`. The Supervisor resolves
it at start, and the App's stored options never hold the value.

No credential is in the image. The final stage copies the binary, the built
frontend and the start script; every key arrives at start from the options or
from the token the Supervisor injects, and `.dockerignore` keeps `.env` out of
the build context altogether.

## Notes

The panel is for administrators: it switches lights and talks through
cameras. The container publishes no port; it is reached through ingress only.
Live camera video is WebRTC from the browser to Home Assistant directly where
the network allows and HLS through the panel where it does not.
