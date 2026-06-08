# fleet-midi-velocity

_Velocity curves — how hard do you hit?_

_One of 16 ternary MIDI agents in the [Live Paradigm Fleet](https://github.com/SuperInstance/sailor-workspace)._

---

## Philosophy — Why Ternary?

The Live Paradigm treats musical gestures as ternary operations. Where binary logic
gives yes/no, ternary gives **approve/reject/observe** — a richer cognitive substrate
that maps naturally to music theory, emotional tension, and conversational flow.

This agent implements **ternary decomposition for velocity**.

## Architecture

Position in the fleet pipeline:

```
🎤 Voice → OpenSMILE (25 features) → Ghost Track (T-0..T-4 CR predictions)
  → tminus-dispatcher (cue scheduling) → Fleet Conductor (routing)
  → velocity (port 2171)
```

## API Reference

| Method | Path | Description |
|--------|------|-------------|
| GET | /health | Health check + agent identity |
| POST | /agent with `{"type":"probe"}` | Liveness probe for fleet-conductor |
| POST | /agent | Process musical data, return ternary analysis |
| POST | / | Direct query with JSON body |

### Response Format

```json
{
  "status": "ok",
  "agent": "fleet-midi-velocity",
  "port": 2171,
  "ternary_vector": [0, 0, 0],
  "ternary_invariant": 0,
  "closed_gesture": false
}
```

## Ternary Logic

| Position | +1 | 0 | -1 |
|----------|------|------|------|
| ternary[0] | accented/hard | neutral | ghosted/soft |

## Educational Supplement

Velocity in MIDI (0-127) represents how hard a note is played. It's one of the most
expressive parameters available — it controls volume, timbre, and articulation.

### Velocity Layers
- **0**: Note off (silent)
- **1-31**: Piano (ghost notes, very soft) — ternary -1
- **32-63**: Mezzo-piano to mezzo-forte — ternary 0
- **64-95**: Mezzo-forte to forte — ternary 0
- **96-127**: Fortissimo (loudest, accented) — ternary +1

### Humanization
Humans never play at exactly the same velocity twice. This agent can apply
randomization (±5-15%) to create more natural-sounding velocity curves.

## Fleet Integration

- **Port**: 2171
- **Roles**: note, cc
- **Conductor ID**: `velocity`
- **Protocol**: HTTP POST to `/2171/agent` with JSON body, 5s timeout
- **Conservation Law**: Σ(Δ_midi) = 4 × Σ(ternary) — closed gestures return to start

## Starting

Local development:

```bash
python3 engine.py --port 2171
```

Or via the fleet start script:

```bash
./scripts/start-fleet-agents.sh
```

## Credits

**Part of the Live Paradigm Fleet** — A ternary cognitive architecture for musical AI.
GitHub: github.com/SuperInstance
Fleet conductor: [sailor-workspace](https://github.com/SuperInstance/sailor-workspace)
