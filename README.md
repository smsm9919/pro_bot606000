# Render Pack for Your DOGE Bot

This pack contains **only infra files** (no strategy code touched) to run smoothly on Render.

## Files
- `render.yaml` — default start with `python -u main.py` (shows live logs, starts trading thread).
- `render.production.yaml` — alternative using `gunicorn` (production-grade WSGI).
- `requirements.txt` — dependencies (includes `gunicorn`).
- `.env.example` — sample env file for local run.
- `PATCH_NOTES.md` — quick checklist & fixes.

## How to use
1. Put these files in the **root** of your GitHub repo next to `main.py`.
2. On Render → New → Blueprint → select your repo (branch: main).
3. Add env vars in Render: `BINGX_API_KEY`, `BINGX_API_SECRET`.
4. Deploy.

### Port binding
Your `main.py` must bind to `PORT` environment variable:

```python
import os
port = int(os.getenv("PORT", 8080))
app.run(host="0.0.0.0", port=port)
```

If your file already uses `app.run(..., port=8080)` it will still work because we set `PORT=8080` in `render.yaml`, but the code above is recommended.

### Pick one start method
- Use `render.yaml` for simplicity.
- If you prefer production server: rename `render.production.yaml` to `render.yaml`.

### Health check
`healthCheckPath: /` — keep your dashboard route `/` responding 200.
