[← Back to README](../README.md)

# 🧪 Testing

- `pytest-django` unit tests for the [middleware](modules.md#middleware), [context processor](context.md), and [health view](health.md) against a minimal Django test project.
- Contract tests (from `greentechhub-core`) run against this package's `IdentityProvider` wiring and health view — `greentechhub-core`'s contract-test base classes exist specifically to catch adapter packages drifting from the shared contract.
- GitHub Actions: lint + test.
