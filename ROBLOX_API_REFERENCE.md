# ROBLOX_API_REFERENCE.md

Authoritative Roblox API reference guide for all Roblox development performed in this repository.

**Last reviewed:** 2026-11-03
**Status:** Active — consult before generating or modifying any Roblox script.

---

## 1. Official Documentation Sources

Use these as the source of truth. Distinguish Roblox Engine APIs (in-experience, runs in Studio/player) from Roblox Open Cloud APIs (external HTTP REST APIs for account/place management, data, moderation, etc.).

| Source | URL | Scope |
|--------|-----|-------|
| Engine API Reference | https://create.roblox.com/docs/reference/engine | Roblox Engine classes, services, properties, methods, events, callbacks, enums |
| Engine API machine-readable index | https://create.roblox.com/docs/reference/engine/llms.txt | LLM-friendly index of the Engine API Reference |
| Complete Creator Documentation index | https://create.roblox.com/docs/llms.txt | Full Creator Hub documentation index |
| Luau documentation | https://create.roblox.com/docs/luau | Luau language syntax, standard library, type system |
| Open Cloud API Reference | https://create.roblox.com/docs/cloud | Roblox Open Cloud REST APIs (external, HTTP-based) |

**Important:** Engine APIs and Open Cloud APIs are disjoint. Engine APIs run inside Roblox Studio/experiences via Luau. Open Cloud APIs are called over HTTPS from external servers. Do not mix them.

---

## 2. Mandatory API Verification Rules

Every time you generate Roblox code, you must:

1. **Verify unfamiliar or important APIs** against current official documentation before use.
2. **Check existence** — confirm that the class, method, property, event, callback, or enum actually exists in the current Engine API Reference.
3. **Check deprecation** — confirm the API is not deprecated, and if it is, use the documented replacement.
4. **Verify signatures** — confirm method arguments, return values, and expected object types match the documentation.
5. **Check execution context** — confirm whether the API runs on client, server, or both, and any `RunContext` restrictions.
6. **Check permissions and security** — confirm required permissions, `LocalScript` vs `Script` requirements, `FilteringEnabled` implications, and remote-event validation needs.
7. **Never invent API names** — do not assume an API exists because it appeared in older tutorials or training data.
8. **Prefer current documentation** — use currently documented APIs over outdated examples.

---

## 3. API Freshness

- Treat official Roblox documentation as the source of truth.
- This file does **not** contain every Roblox API. Follow documentation links to the specific class or method being used.
- Record the date this reference file was last reviewed (see header).
- Never claim an API is current without verifying it against the official Engine API Reference.

---

## 4. Coding Standards (Luau)

When generating Luau:

- Use current Luau syntax (Luau 5.1-compatible as implemented by Roblox).
- Prefer type annotations where useful (`local x: number = 0`, function signatures, module types).
- Use appropriate client/server separation — `LocalScript` for client, `Script` for server, `ModuleScript` for shared logic.
- Validate all remote communication on the server (never trust the client).
- Avoid deprecated APIs — check the Engine API Reference for deprecation notices.
- Handle errors and edge cases — use `pcall`/`xpcall` where appropriate, validate inputs.
- Avoid unnecessary scripts and duplicated logic — prefer `ModuleScript` and shared modules.

---

## 5. Documentation Priority

When sources disagree, use this priority order:

1. Current official Roblox Engine API Reference (`create.roblox.com/docs/reference/engine`).
2. Current official Roblox Creator documentation (`create.roblox.com/docs`).
3. Official Roblox announcements and release notes.
4. Other sources (tutorials, forums, training data) only as supplementary information — never as authority.

---

## 6. Future Development Instructions

This document must be consulted before generating or modifying Roblox scripts.

For each significant implementation:

1. Identify every Roblox API (class, service, method, property, event, enum) you plan to use.
2. Verify each API exists in the current Engine API Reference.
3. Confirm none are deprecated; note any replacements.
4. Confirm client/server execution requirements for each API.
5. Report any deprecated APIs or compatibility concerns discovered.
6. Record findings in this file if they represent a generalizable pattern.

---

## 7. Known Limitations

- **Live documentation verification:** This sandbox cannot reach `create.roblox.com` (outbound network is restricted to GitHub, npm, and PyPI hosts). The APIs and patterns below are recorded from general knowledge and must be re-verified against the live Engine API Reference before use in production code.
- **Not exhaustive:** This file is a concise index, not a copy of the entire Roblox API.

---

## 8. Quick Reference Index

### Core Services (Engine API)

| Service | Purpose | Typical Location |
|---------|---------|------------------|
| `Players` | Player management, local player, character spawning | Server/Client |
| `Workspace` | 3D world, parts, terrain, camera | Server/Client |
| `ReplicatedStorage` | Shared assets accessible client and server | Server/Client |
| `ServerStorage` | Server-only assets | Server |
| `StarterPlayer` / `StarterPlayerScripts` / `StarterCharacterScripts` | Player/character starter scripts | Client |
| `StarterGui` | ScreenGuis, HUD elements | Client |
| `Lighting` | Lighting, atmosphere, sky, time of day | Server/Client |
| `RunService` | Heartbeat, RenderStepped, Stepped, context detection | Server/Client |
| `HttpService` | Outbound HTTP requests (allowlist required) | Server (client limited) |
| `DataStoreService` | Persistent player data | Server |
| `MemoryStoreService` | Ephemeral cross-server data | Server |
| `TeleportService` | Teleporting players between places | Server |
| `MarketplaceService` | Developer products, game passes, prompts | Server/Client |
| `BadgeService` | Awarding badges | Server |
| `ReplicatedFirst` | Pre-load assets before replication | Client |
| `CollectionService` | Tagging instances for systems | Server/Client |
| `ContextActionService` | Input binding (keyboard, gamepad, touch) | Client |
| `UserInputService` | Raw input detection | Client |
| `GuiService` | GUI modality, safe area, core UI | Client |
| `TweenService` | Property interpolation | Server/Client |
| `PathfindingService` | NPC pathfinding | Server |
| `SoundService` | Sound groups, effects | Server/Client |
| `Debris` | Automatic instance cleanup | Server/Client |
| `ChangeHistoryService` | Studio plugin undo/redo | Studio plugins |
| `Selection` | Studio selection | Studio plugins |

### Common Classes

| Class | Notes |
|-------|-------|
| `Part`, `MeshPart`, `BasePart` | 3D geometry; `Anchored`, `CanCollide`, `CanTouch`, `CastShadow`, `CollisionFidelity` |
| `Model` | Container for parts; `PrimaryPart`, `Model:PivotTo()` |
| `Tool` | Equippable items; requires `Handle` part; `Tool.Grip`, `Tool.Activated` |
| `Humanoid` | Character health, state, movement; `Humanoid.Animator` |
| `Animator` | Loads and plays `Animation` tracks |
| `Animation` | `rbxassetid://` reference; loaded via `Animator:LoadAnimation()` |
| `KeyframeSequence` | Raw animation data inserted into `ServerStorage` |
| `SurfaceAppearance` | PBR material overrides on `MeshPart` |
| `ProximityPrompt` | Interaction prompts; `ProximityPrompt.Triggered` |
| `RemoteEvent`, `RemoteFunction` | Client-server communication; validate on server |
| `BindableEvent`, `BindableFunction` | Same-context (client-client or server-server) communication |
| `Script` | Server-side Luau |
| `LocalScript` | Client-side Luau |
| `ModuleScript` | Shared code; `require()` |
| `StringValue`, `IntValue`, `NumberValue`, `BoolValue`, `ObjectValue` | Legacy value objects; prefer attributes |

### Common Enums

| Enum | Examples |
|------|----------|
| `Enum.KeyCode` | `W`, `A`, `Space`, `ButtonX`, etc. |
| `Enum.UserInputType` | `Keyboard`, `MouseButton1`, `Touch`, `Gamepad1` |
| `Enum.Priority` | `Action`, `Movement`, `Idle`, `Action2`, `Action3`, `Action4`, `Action5` |
| `Enum.CollisionFidelity` | `Default`, `Box`, `Hull`, `PreciseConvexDecomposition` |
| `Enum.HumanoidStateType` | `Running`, `Jumping`, `Freefall`, `Landed`, `Swimming`, etc. |
| `Enum.StickOffset` | For UI anchoring |

### Luau Language

| Feature | Guidance |
|---------|----------|
| Type annotations | `local x: number`, `function f(a: string): boolean` |
| Type aliases | `type Point = {x: number, y: number}` |
| Generics | `function map<T, U>(t: {T}, f: (T) -> U): {U}` |
| `continue` | Supported in loops |
| `table.insert`, `table.remove`, `table.create`, `table.pack`, `table.unpack` | Standard |
| `task.wait()`, `task.spawn()`, `task.delay()` | Preferred over `wait()`, `spawn()`, `delay()` |
| `pcall`, `xpcall` | Error handling |

---

## 9. Verification Checklist (per implementation)

Before committing any Roblox script, confirm:

- [ ] Every class/service used exists in the current Engine API Reference
- [ ] No deprecated APIs are used (or replacements documented)
- [ ] Client/server execution context is correct for each API
- [ ] Remote events are validated on the server
- [ ] Type annotations used where helpful
- [ ] Error handling (`pcall`) used for risky calls (`HttpService`, `DataStoreService`, teleport)
- [ ] Documentation priority followed if sources disagree

---

*End of ROBLOX_API_REFERENCE.md*