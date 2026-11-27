# razdor-openmw

Converter from a Discord Times map to a playable OpenMW world.

Input: the JSON that Razdor's `razdor-dtm-json` writes from a `.DTm` map (format `razdor-dtm-json`,
version 1; layout in Razdor's `docs/flows/testing-and-tools.md`, code in `src/dt/dtm_json.rs`,
branch `dtm-json-2026-11-27`).

Output: an OpenMW content file built on the public-domain OpenMW Game Template
(https://openmw.readthedocs.io/en/stable/reference/modding/openmw-game-template.html), so no
Morrowind files are needed. Terrain is written offline (OpenMW Lua cannot create land); Lua
handles quests and events at runtime.

Rules:
- Never commit anything from Discord Times (maps, JSON exported from real maps, texts, art).
- Models are our own or CC0, brought in through Blender as Collada or NIF.
