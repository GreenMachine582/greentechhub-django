[← Back to README](../README.md)

# 🏷️ Versioning & Distribution

- Own repo (`GreenMachine582/greentechhub-django`), semver git tags, `pip`/`uv` git installs.
- Depends on a pinned `greentechhub-core` version range, tracked independently — not assumed to move in lockstep with `greentechhub-core` releases.
- Given there's currently exactly one Django consumer (GreenTechHub), breaking changes here have a small blast radius. **Non-breaking**: new optional settings fields, new context keys, new hooks. **Breaking**: changing the settings shim's expected env vars, removing or renaming an existing context-processor key, changing an existing bridge's expected input shape.
