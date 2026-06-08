# fleet-midi-velocity: Theory

_Musical theory and ternary logic behind the velocity agent._

---

## Velocity Curves and Expression

A velocity curve maps MIDI velocity values (0-127) to actual loudness. Different
instruments and playing styles use different curves:

- **Linear**: velocity 64 = 50% loudness. Default, but not very expressive.
- **Exponential**: velocity 64 = 30% loudness. More dynamic range at the top end.
- **Logarithmic**: velocity 64 = 70% loudness. Compressed, even-loudness feel.

This agent accepts a `curve` parameter to specify the desired curve type.

---

_This document is part of the educational supplement for [fleet-midi-velocity](https://github.com/SuperInstance/fleet-midi-velocity)._
