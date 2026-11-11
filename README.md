# Merge Grid

A 2048-style puzzle: **React + Vite** frontend, optional **Python (FastAPI)** backend.

The game is fully playable without Python. When the API is running, two extras appear:
a **Hint** button (expectimax solver in `server/solver.py`) and a persistent **Leaderboard** (SQLite).

## Run it

```bash
# 1. frontend
npm install
npm run dev            # http://localhost:5173

# 2. backend (second terminal, optional)
python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt
npm run api            # http://127.0.0.1:8000  (Vite proxies /api to it)
```

Single-process mode: `npm run build`, then `python3 -m uvicorn server.main:app --port 8000`
serves the built app and the API together at http://localhost:8000.

## Tests

```bash
npm test                                   # game logic (Vitest)
pip install -r requirements-dev.txt && python -m pytest   # solver + API
```

## Layout

| Path | What |
|---|---|
| `src/game.js` | Pure game logic: move/merge/spawn, win/lose, undo snapshots |
| `src/App.jsx` | State (useReducer), input (keys, WASD, swipe), style switcher, hint |
| `src/components/` | `Board`, `Overlay` (win/game over + save score), `Leaderboard` |
| `src/api.js` | Fetch client; if the API is down the online features just hide |
| `server/solver.py` | Expectimax with memoised row moves + heuristic (depth 2-4, adaptive) |
| `server/main.py` | `GET /api/health`, `GET /api/leaderboard`, `POST /api/scores`, `POST /api/hint` |
| `legacy/merge-grid-original.html` | The original single-file version |

Scores are submitted by the browser, so the server only sanity-checks them (score must be
plausible for the best tile); it is not anti-cheat.
# merge-grid
