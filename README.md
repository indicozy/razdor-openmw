# razdor-openmw

This tool turns a *Discord Times* (Времена раздора) map into a world that you can play in
[OpenMW](https://openmw.org). It runs on the free OpenMW Game Template. You do not need Morrowind.

The tool is an experiment. It comes from the [Razdor](https://github.com/indicozy/razdor) project.
Development happens on Razdor's `openmw-2026-11-27` branch. This repository is a public copy of
that branch's OpenMW parts.

## What it does

- Terrain: heights, ground textures, water, the map edge and passability, from the DT map.
- Scenery and buildings: forests, mountains, towns and castles as simple generated meshes.
- Armies: each unit card is its own actor. Squads walk together, patrol and attack on sight.
- Events: map events in Lua, with windows, questions, journal entries, gold, mana, units,
  artifacts, quests, the hall list, victory and defeat.
- Spells: SPEL records for the hero's spells and for unit strikes, heals and shots.
- Real-time fights: a Lua fight driver deals melee, shots and spell strikes from DT's unit
  numbers. A synthetic 4 vs 4 test passes.

## What it does not do yet

- DT's damage rules, surrender and the battle wrap-up (milestone M9).
- The hero's party as followers, and the world freeze during a fight.
- Most of the economy, the strategic AI, ships and town windows. See
  `docs/specs/2026-11-28-openmw-dt-rules-gap.md`.
- Real models. The units are block figures with no animation.

## Layout

- `crates/openmw-world`: the converter (`razdor-openmw`, Rust) and its Lua runtime.
- `tools/openmw-smoke`: off-screen checks in OpenMW 0.51 (`play.sh`, `run.sh`, and the m5 and m8
  scenarios).
- `docs/specs`: the design spec and the gap list.

## Use

You need the map exporters from Razdor (`razdor-dtm-json`, `razdor-units-json`) and your own
copy of the game. This repository holds no game files. The converter reads the JSON that the
exporters write.

```
cargo build --release
razdor-openmw <map.json> -o <out dir> --units <units.json> --template <template.omwgame>
```

`tools/openmw-smoke/README.md` explains how to run the result in OpenMW.

## Licence

GPL-3.0-only, the same as Razdor. *Discord Times* and its data belong to their authors.
