# House — Home Assistant App repository

The App **metadata only**, for the Supervisor to read: `repository.yaml` and
`house/` (`config.yaml`, `DOCS.md`, `CHANGELOG.md`). The source, the
Dockerfile and the build live in the private repository
`CaryLandholt/home-assistant-toolkit` under `ha-app/`, and the image is
private on GHCR. This copy exists because the Supervisor fetches an App
repository with a plain `git clone`, which a private repository refuses.

Sync these files from `ha-app/` on every version bump; the Supervisor shows
whatever version `house/config.yaml` names, and the image for it must exist.

Add to Home Assistant: Settings → Apps → App store → ⋮ → Repositories →
`https://github.com/CaryLandholt/house-ha-app`. The image needs `ghcr.io`
under ⋮ → Registries with a GitHub token carrying `read:packages`.
