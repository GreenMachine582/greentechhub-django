[← Back to README](../README.md)

# ⚙️ Settings Shim

```python
# greentechhub_django/settings.py
def load_gth_settings() -> GTHBaseSettings:
    """Reads the same env vars greentechhub-core's GTHBaseSettings expects,
    so GreenTechHub's .env looks like every FastAPI service's .env."""
    return GTHBaseSettings()

# GreenTechHub's settings.py
from greentechhub_django.settings import load_gth_settings
gth = load_gth_settings()
SECRET_KEY = gth.secret_key
LOG_LEVEL = gth.log_level
```

Django's settings system is import-time and global in a way FastAPI's dependency-injected `Settings` isn't — this shim exists specifically to bridge that difference once, rather than every Django service (currently just GreenTechHub, but not necessarily forever) re-deriving env var names independently and drifting from the FastAPI services' naming.
