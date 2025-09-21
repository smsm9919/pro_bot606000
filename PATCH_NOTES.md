# PATCH NOTES (Infra only — no trading logic touched)

## 1) Start command
- `python -u main.py` ensures **trading thread starts** and logs are unbuffered.

## 2) Gunicorn option
- Provided `render.production.yaml` with `gunicorn -b 0.0.0.0:$PORT main:app`.
- If you use this, ensure your bot thread starts on import or via `@app.before_first_request`.
  Example (safe, optional):
  ```python
  started = False
  @app.before_first_request
  def _start_bot():
      global started
      if not started:
          from threading import Thread
          t = Thread(target=main_bot_loop, daemon=True)
          t.start()
          started = True
  ```

## 3) Environment
Add on Render:
- `BINGX_API_KEY`
- `BINGX_API_SECRET`
- (Optional) `PYTHONUNBUFFERED=1`

## 4) Uptime
- Free plan spins down; keep a monitor (UptimeRobot) hitting `/` every 5 min.
