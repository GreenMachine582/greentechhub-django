[← Back to README](../README.md)

# 🔌 Template Context Processor

This is the mechanism behind GreenTechHub getting the same navbar, cards, and modals as other adaptors, without migrating off Django. Read this first if you're only reading one doc.

```python
# greentechhub_django/context_processors.py
def gth_context(request):
    return {
        "nav_items": get_nav_items_for(request),
        "current_user": getattr(request, "gth_identity", None),
        "flashes": messages_to_flash(get_messages(request)),
        "brand": brand_context(),
    }
```

Registered in Django's `TEMPLATES[...]["OPTIONS"]["context_processors"]`, this is exactly the context shape `greentechhub-ui`'s macros expect (its template context contract) — supplied automatically on every request instead of GreenTechHub's views building it manually each time.

`current_user` comes from `request.gth_identity`, set by the [auth bridge](auth.md); `flashes` comes from the [messages bridge](modules.md#messages-and-events).
