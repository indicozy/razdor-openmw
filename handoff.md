# Handoff 2026-11-28: OpenMW world, M8 (spells and the 4 vs 4 fight)

Goal (the owner): one proper 4 vs 4 fight in the OpenMW world, then DT's rules so that any map works.
Source of truth: Razdor's branch `openmw-2026-11-27` (head 1e0df139, not merged into main).
This repository is a copy of the OpenMW parts of that branch.

## Done this session

- Units exporter v2 with the world spells (`razdor-units-json`, in Razdor).
- Spec M8 (spells, the fight), M8 spike notes, world freeze rule, M9 (DT battle rules around the
  fight), in `docs/specs/2026-11-27-openmw-world.md`.
- Converter: SPEL records (world spells `rz_spell_<id>`, unit strike/heal/bless/shot per unit and
  level), NPCS on caster CREAs, `scripts/razdor/combat_data.lua` (per body: melee, shot, strike,
  heal, bless, interval). 105 crate tests pass, clippy clean.
- Lua fight driver `crates/openmw-world/lua/fight.lua`: hits come from Lua (`hit()` is the one
  damage function), movement from OpenMW's AI packages. Smoke `tools/openmw-smoke/m8/run_m8.sh`
  PASS: 19 melee, 27 shot, 19 strike hits, one side wiped at about 38 s.
- Gap list of every DT rule a map can see: `docs/specs/2026-11-28-openmw-dt-rules-gap.md`
  (about 10 done, 39 partial, 59 missing; milestones M10 to M19 proposed).

## Findings that shape the design

- OpenMW's own AI does not attack or cast with the block figures (no animation). With NPC_ records
  on the Template's animated body (BasicPlayer, groups for attack, bow and spellcast), swings and bow
  shots animate, but no damage lands and casters still never cast. Hits stay in Lua either way,
  which DT's damage rules need anyway.
- The owner: Morrowind models are the later target ("use Morrowind models, for now stay in
  template"). Ranged units use the cast path ("for ranged use mage attack animation").
- Clock: the converter's `--time-scale` defaults to 120 (one game day = 720 s). The M8 spec text
  says 30; the code uses the real clock. Settle it in M13 (time model).

## Next, in order

1. M8 part 2: the party as follower bodies (spec 3.9), closed sides and the world freeze (spec
   "a fight freezes the rest of the world"), the hero's magic menu (`types.Actor.spells(player):add`),
   one mana pool (Lua `s.mana` written to the player's magicka and read back), `cast_spell` on the player.
   A healer unit in the m8 scenario so heal and bless are tested.
2. M9: DT's damage, surrender, turn limit, XP and loot in `fight.lua`'s `hit()` and a wrap-up,
   ported from Razdor's `src/rules` with the original's addresses; units JSON v3 with `surrender`
   and an `options` block.
3. M10 onwards from the gap list (data export v2 first).
4. Later: animated bodies (Template body or Morrowind models) with Lua playing the attack groups;
   check first that a local script can play a group in OpenMW 0.51.

## Scratch (outside git, on the owner's machine)

`/mnt/ai_models/razdor-scratch/openmw/`: `spike-cast/`, `spike-npc/` (spike evidence),
`m8-rust/`, `m8-lua/run3/` (fight log and pictures), `target-spells/` (build dir, can be removed).
