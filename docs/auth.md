[← Back to README](../README.md)

# 🔐 Auth Bridge

```python
# greentechhub_django/auth/middleware.py
class GTHAuthMiddleware:
    def __init__(self, get_response):
        self.get_response = get_response

    def __call__(self, request):
        provider = get_configured_provider()   # local or forward_auth, from settings
        identity = provider.resolve_sync(RawAuthContext(
            cookie=request.COOKIES.get("gth_session"),
            headers=request.headers,
        ))
        request.gth_identity = identity          # framework-independent Identity
        request.user = to_django_user(identity)  # adapted for code that expects request.user
        return self.get_response(request)
```

`greentechhub-core`'s `IdentityProvider` implementations (`DevelopmentIdentityProvider` now, `AuthentikIdentityProvider` once a reverse proxy fronts GreenTechHub with an Authentik outpost) — swapping from local dev auth to Authentik is a one-setting change, not a GreenTechHub-specific reimplementation.

`resolve_sync()` is `greentechhub-core`'s sync wrapper around `resolve()`, added specifically so Django's traditionally-synchronous middleware model doesn't need to go async just to resolve identity — not a Django-side workaround.

`request.gth_identity` is what the [template context processor](context.md) reads for `current_user`.
