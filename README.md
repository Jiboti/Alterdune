<h1 align="center">⚔️ Alterdune</h1>
<h4 align="center">A turn-based roguelike RPG in C++ — kill or spare your way through 10 fights toward one of three endings · OOP project, ESILV</h4>

<p align="center">
  <img width="1147" height="833" alt="image" src="https://github.com/user-attachments/assets/2f8042de-1155-4e06-9fe9-6d5630feddc9" />

</p>

---

## 🎮 Overview

**Alterdune** is a turn-based roguelike RPG built in **C++17** with **SFML** and **ImGui**. The player goes through **10 consecutive fights**, choosing at every encounter between two paths: **kill** the monster (the *Conquest* path) or **spare** it through the **Mercy** system (the *Redemption* path). The mix of choices across a run leads to one of **three different endings** — Conquest, Redemption, or Balance.
Contributor: https://github.com/Antoine-Roucau/Antoine-Roucau

## ✨ Features

- **10 fights per run**, with monsters that scale up as you progress (+5% per fight, +50% more on the hard path).
- **Two moral paths & a Mercy system** → three distinct endings depending on how many enemies you kill vs spare.
- **3 character classes** (Berserker, Guardian, Trickster), each with three exclusive once-per-fight powers.
- **Equipment across 9 slots** plus a consumable inventory.
- **Level-up with stat choices**, an XP/gold economy, in-run loot and a shop.
- **Persistent save with mid-run resume** — automatic save after each fight, with corrupt-save auto-cleanup.
- **Full SFML + ImGui interface**: 8 screens, a colored combat log, fade transitions, a hidden bestiary (unseen monsters shown as `???`), and an unlimited run history.
- **Fully data-driven**: 100 actions, 100 items and 100 monsters are scripted in **CSV** — game content can be changed without recompiling.

<p align="center">
  <img width="1815" height="992" alt="image" src="https://github.com/user-attachments/assets/a6c63b82-1fe2-4b7d-bb80-be902ab32168" />
</p>

## 🧩 Under the hood — OOP design

As an Object-Oriented Programming project, the architecture is built around clean OOP principles:

- **Abstract base class** `Entity` (with a pure-virtual `playTurn()`), specialized into `Player` and `Monster`.
- **Polymorphism** in `Monster::playTurn()`, which switches between three AI behaviours (*aggressive*, *defensive*, *random*).
- An **action system** of 17 types — instant effects, 3-turn over-time effects (handled by `tickEffects()`), and conditional effects.
- A **strict separation between logic and rendering** (MVC-style): the `Game` class holds *all* state and game logic and does no drawing, while `Interface` only reads that state to render it and never writes to it directly.
- **Data-driven loading** of the four CSV files at startup, each wrapped in its own error handling so a malformed file can't bring down the others.

### Character classes

| Class | HP | ATK | DEF | Playstyle |
|---|:---:|:---:|:---:|---|
| Berserker | 80 | 12 | 3 | High-damage aggression (Rampage, Fury, Armor Break) |
| Guardian | 120 | 6 | 10 | Tanky defense (Fortify, Iron Skin, Endure) |
| Trickster | 90 | 7 | 5 | Fast on the Mercy path (Hex, Mind Link, Enfeeble) |

## 🛠️ Tech stack

`C++17` · `SFML 3.0.2` · `ImGui-SFML` · `CSV`

## 📁 Project structure

```
main.cpp                     Entry point — creates Game + Interface, runs the loop
game.h / game.cpp            Business logic (state, combat, save)
interface.h / interface.cpp  ImGui/SFML rendering & events
entity / player / monster    OOP hierarchy (abstract base + derived classes)
action / item / inventory    Actions, items and inventory systems
Ressources/CSV/              actions.csv, items.csv, monsters.csv, run.csv (game content)
Save/                        Persistent save (save.txt)
```

## ▶️ Build & run

Requires **C++17**, **SFML 3.0.2** and **ImGui-SFML**. Build with your toolchain, then run the executable with the `Ressources/` and `Save/` folders alongside it.

A prebuilt Windows build is available in the **Releases** section.

📄 A full technical documentation (architecture, action system, CSV formats, save format) is available in [`docs/Documentation_Alterdune.pdf`](docs/Documentation_Alterdune.pdf).

---

<p align="center"><a href="https://github.com/Jiboti">⬅️ Back to profile</a></p>
