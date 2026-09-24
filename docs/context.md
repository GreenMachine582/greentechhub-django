[← Back to README](../README.md)

# 🔌 Template Context Processor

This is the mechanism behind GreenTechHub getting the same navbar, cards, and modals as other adaptors, without migrating off Django. Read this first if you're only reading one doc.

```python
# greentechhub_django/context_processors.py
def gth_context(request):
    return {
        "nav_items": get_nav_items_for(request),
        "current_user": getattr(request, "gth_identity", None),
        "current_path": request.path,
        "flashes": messages_to_flash(get_messages(request)),
        "brand": brand_context(),
    }
```

Registered in Django's `TEMPLATES[...]["OPTIONS"]["context_processors"]`, this is exactly the context shape `greentechhub-ui`'s macros expect (its template context contract) — supplied automatically on every request instead of GreenTechHub's views building it manually each time.

`current_user` comes from `request.gth_identity`, set by the [auth bridge](auth.md); `flashes` comes from the [messages bridge](modules.md#messages-and-events). `current_path` is what `gth_sidebar` / `gth_navbar` mark active and `nav_breadcrumbs` resolves — `greentechhub-fastapi`'s `templating.ui_context` supplies the same key on the FastAPI side.

## Wiring greentechhub-ui into the Jinja2 backend

`greentechhub-ui` depends on `jinja2` only, and ships framework-neutral helpers for this (its `docs/contract.md`, "Setup") — nothing Django-specific needs to live in `greentechhub-ui`:

```python
# greentechhub_django/jinja2.py — TEMPLATES[...]["OPTIONS"]["environment"] points here
from jinja2 import Environment
import greentechhub_ui

def environment(**options):
    # gth-ui's templates load after the project's own; shell_globals() installs
    # brand, the asset URLs and the nav helpers (nav_breadcrumbs, …).
    return greentechhub_ui.install(Environment(**options), service_name="GreenTechHub",
                                   nav_items=[...])

# settings.py — gth-ui's static files under the same prefixes shell_globals() renders URLs for
STATICFILES_DIRS = [(p.strip("/"), d) for p, d in greentechhub_ui.static_dirs().items()]
```

(`greentechhub_ui.template_dirs()` is the alternative to `install()` when the project would rather list gth-ui's directories in `TEMPLATES[...]["DIRS"]` and set globals itself.) For htmx views, `greentechhub_ui.htmx.wants_fragment(request.headers)` / `hx_target(...)` work on Django's `request.headers` unchanged, and a toast is `HttpResponse(status=204, headers={"HX-Trigger": greentechhub_ui.toast("Saved")})`. `greentechhub-ui`'s own test suite renders `app.html` through `django.template.backends.jinja2.Jinja2` with exactly this `environment` callable.
