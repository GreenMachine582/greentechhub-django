[← Back to README](../README.md)

# 🩺 Health View

```python
# greentechhub_django/health/views.py
async def health(request):
    results = await run_checks([check_database, check_disk])
    return JsonResponse(render_health_result(results))
```

Uses `greentechhub-core`'s check functions and `HealthResult` type directly — GreenTechHub's `/health` reports in the same shape as every other service running the same checks, since none of them own the check logic themselves.
