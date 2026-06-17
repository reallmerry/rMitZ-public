[🇷🇺 Русский](README_RU.md) · **🇬🇧 English**

# Drones

A complete combat UAV system for Paper servers 1.21.x: autonomous strike drones
with real ballistics and navigation, pilot-controlled FPV kamikaze drones, recon
drones and electronic-warfare (jamming) stations.

These drones don't just "fly straight to a point". They plan a route, fly around
buildings, hold altitude over a city, line up on the target and dive — like the
real thing.

Author: **reallmerry**

---

## Purchase & Contact

The plugin is available for purchase.

* Website: **reallmerry.store**
* Telegram: *(your contact)*
* Discord: *(your contact)*
* Test server and screenshots — on request

---

## How it works (in plain terms)

The idea is simple: a player gets a deploy kit, places a drone in the world and
launches it. After that the drone acts on its own — or a player flies it, if it's
an FPV.

* **Strike drones** fly to the given coordinates, avoid obstacles by themselves
  and detonate on target.
* **FPV and recon** are flown in first person — mouse and WASD.
* **EW station** is placed like a block and jams enemy drones in a radius.

All visuals, models, sounds, power and radii are configurable.

---

## Drone types

**Shahed-136** — a slow strike drone. Launched by coordinates: it climbs, cruises
over the buildings, dives and explodes. The target can be changed mid-flight.

**Lyuty** — bigger and more agile than the Shahed, flies higher and faster with
sharper turns.

**FPV kamikaze** — a light first-person drone. The operator "sits" inside it:
mouse — heading, WASD — thrust, Space/Shift — up/down, F — detonate. The frame
behaves like a quadcopter: it banks in turns and noses down on throttle. Rams the
target.

**Recon drone** — flown like an FPV but with no warhead. It highlights nearby
enemies — the whole team sees the mark through walls. Bigger battery and range,
quieter engine. Makes a clean "scout → designate → strike" loop.

**Guided drone** — flies on its own but goes where the operator looks. You lead
the target with your gaze, the drone turns onto it and dives.

**EW station** — jams drones in a radius. Controlled drones lose signal and fall.
An autonomous Shahed won't fall, but it starts losing course accuracy. The station
can be picked up or shot down.

---

## Navigation & flight algorithms

This is the heart of the plugin — drones fly by physics and pathfinding, not by a
script.

* **Pathfinding.** Before launch the drone builds a map of the terrain and finds
  the shortest free route to the target — it decides on its own whether to go
  around a building or fly over it. The search runs on a background thread, so a
  launch (even a full salvo) doesn't freeze the server.
* **Plane-style turns.** The drone doesn't spin in place — it banks and flies an
  arc whose radius grows with speed. It looks natural.
* **Flight over a city.** Altitude stays steady: the drone climbs quickly before
  an obstacle and descends slowly behind it, so it doesn't yo-yo over rooftops.
* **Obstacle avoidance.** The drone "looks" ahead and climbs in an emergency if a
  wall appears in front.
* **Flight phases.** Climb → route → terminal dive.
* **Aerodynamics.** Drag, inertia, roll and light turbulence make the trajectory
  feel alive rather than perfectly mathematical.

---

## Combat & damage

* **Explosion with block damage** (configurable, can be disabled).
* **Concussion.** Players near the blast get blindness, nausea and slowness — the
  closer to the center, the stronger.
* **Damage bypasses region protection.** The drone hits players directly, so it
  works even where a normal explosion is blocked (WorldGuard etc.).
* **Detonation on impact** with a block or a player at speed.

---

## Shooting drones down

* **Any projectile** — arrow, snowball, egg, trident.
* **Weapon plugins** — both projectile-based (QualityArmory) and raytrace
  (WeaponMechanics and similar). Fast bullets are caught along their path, not
  point-by-point.
* **Two ways to fall.** A drone shot down by a projectile falls as a dud — smoke
  and debris, no explosion. A drone jammed by EW falls with a live warhead and
  detonates on impact — placing EW near your own base is risky.

---

## Technical features

* **Visible from far away.** The server "forgets" displays past ~128 blocks. The
  plugin streams the drone over packets, so it's visible up to ~384 blocks
  (configurable).
* **Doesn't vanish mid-flight.** The drone keeps its chunks loaded, but does not
  force-generate new terrain — flying over wilderness won't load the server.
* **ModelEngine 3D models** for all drones and the EW station (optional). Without
  ModelEngine a drone is drawn as an item model.
* **Engine sound** with an approach effect, a separate launch sound, volume =
  audible range.
* **Operator HUD** — FPV goggles with a crosshair and a battery/signal bar.
* **Effects** — a long-range contrail, fire and smoke on the dive, an explosion.

---

## Performance

The plugin is built for a live server, not single-player:

* one shared loop for all drones, not a task per entity;
* pathfinding moved to a background thread;
* protection against generating chunks over unexplored terrain;
* bullet-proximity checks only run while someone is actually shooting;
* a cap on drones in the air.

---

## Commands

| Command | Description |
|---|---|
| `/drone give <type>` | give a deploy kit (shahed / lyuty / fpv / recon / reb) |
| `/drone launch <id> [x y z]` | launch a drone (strike drones need coordinates) |
| `/drone guide <id>` | launch a strike drone aimed by gaze |
| `/drone retarget <id> <x y z>` | change an autonomous drone's target in flight |
| `/drone list` | list drones in the air and deployed |
| `/drone cancel` | remove all drones |
| `/drone reload` | reload the config |

Right-click a block with the kit to deploy a drone. Sneak and hit a drone or
station to pick it back up. Command permission — `atlas.drone`.

---

## Requirements

* **Paper** (or a fork) **1.21.x**
* **Java 21**
* **CommandAPI** — required (commands)
* **packetevents** — required (FPV control and far rendering)
* **ModelEngine** R4.x — optional (3D models)

Installation details — see **[INSTALL.md](INSTALL.md)**.

---

## Honest note on limitations

Minecraft and ModelEngine can't rotate a 3D model on all axes — only by heading.
Because of that:

* **tilt isn't shown on the 3D model** — in a dive or a fall the drone visually
  keeps facing "forward" even though it's going down. From the side this can look
  a bit unusual;
* **perfectly smooth turns** aren't possible — turning happens within what the
  engine allows.

This is a limitation of Minecraft and ModelEngine itself, not the plugin, and it
can't be fully worked around. It doesn't affect gameplay: the drone flies, aims,
hits and gets shot down as it should — the model just "faces" the wrong way while
falling. If tilt matters to you more than the 3D model, every type has a
`me-in-flight: false` switch — then tilt is shown fully, but the drone uses an
item model instead of the ModelEngine one.

---

## What you get on purchase

* The plugin itself (jar)
* A ready archive with 3D models and the resource pack — just unpack it on the server
* Installation and configuration docs
* Updates and fixes
* Direct contact with the author for questions and requests
