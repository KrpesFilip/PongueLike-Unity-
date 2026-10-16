# Ponguelike

**2D Pong Game with Persistent Meta-Progression, Built in Unity 6**

A Pong game for one or two players with an AI opponent, physics-based ball movement and screen-shake, particle and audio feedback. Matches against the AI award points, which are stored between sessions and spent in a shop on cosmetic and gameplay modifiers for the player's paddle and the ball.

## Features

- Two play modes: local Player vs Player and Player vs AI
- AI opponent with a dead zone and randomized speed variance, so it can be beaten
- Ball velocity increases ×1.1 on every paddle hit
- Hit feedback: camera shake scaled to ball speed, particle bursts, trail renderer and SFX
- First to 3 wins (configurable through `GameManager.maxScore`)
- Pause menu (`P`) that freezes the game with `Time.timeScale`, and a master volume slider
- Point economy in PvE: +1 for each goal, +3 for a win, +1 for a loss
- Shop with buy and equip/unequip states, plus a full progress reset
- Background music that keeps playing across scene loads
- Windows and WebGL build targets

## Controls

| Action | Player 1 | Player 2 |
|---|---|---|
| Move up | `W` | `↑` |
| Move down | `S` | `↓` |
| Pause | `P` | `P` |

## Shop items

| Item | Cost | Effect |
|---|---|---|
| Geometric reference | 1 | Replaces the ball sprite with a cube skin |
| The First Paddle | 1 | Alternative sprite for Player 1's paddle |
| GrowthSpurt | 5 | Makes Player 1's paddle 1.5× longer |

## Tech stack

| Area | Technology |
|---|---|
| Engine | Unity 6 (6000.0.41f1), 2D (URP-free, built-in renderer) |
| Language | C# |
| Physics | Unity 2D Physics (`Rigidbody2D`, bouncy `PhysicsMaterial2D`) |
| UI | uGUI, TextMesh Pro |
| Input | Legacy Input Manager (`MovePlayer1` / `MovePlayer2` axes) |
| Visual effects | Particle System, Trail Renderer, Post Processing Stack v2, Animator |
| Persistence | `PlayerPrefs` |
| Build targets | Windows (x86_64), WebGL |

## Architecture

```
StartMenu ──► Gnop (match) ──► Shop
   ▲              │               │
   └──────────────┴───────────────┘   SceneLoader (build index)

Gnop scene
   GameManager (singleton)
     ├── score state, win check, play mode (PvP / PvE)
     ├── onReset event ─────────► Ball, Paddle (reset positions)
     ├── GameUI ─── onStartGame ► Ball (serve), GameManager (clear score)
     ├── GameSFX / ScreenShake
     └── PlayerPrefs ◄────────── points earned in PvE

   Ball ── OnTriggerEnter2D(ScoreZone) ──► GameManager.OnScoreReached
        └─ OnCollisionEnter2D(Paddle | Wall) ──► SFX, particles, shake

Shop scene
   ShopManager ──► ShopItemUI[]  (buy / equip, <item>_bought / <item>_equipped)
        └──► PlayerPoints        (balance, spend, reset)
                    │
                    ▼
               PlayerPrefs ──► read by Ball / Paddle on Start() to apply skins
```

Systems are connected through C# `Action` events (`onReset`, `onStartGame`) rather than direct references. Shop state reaches gameplay only through `PlayerPrefs` keys, which `Ball` and `Paddle` read when the match scene loads.

## Project structure

```
Assets/
├── Scenes/          StartMenu, Gnop, Shop
├── Scripts/
│   ├── GameManager.cs    Match state, scoring, play mode, point rewards
│   ├── Ball.cs           Serve, speed-up, collision handling, ball skin
│   ├── Paddle.cs         Player input, AI movement, paddle skin and length
│   ├── GameUI.cs         Menus, scoreboard, pause, volume, end screens
│   ├── ShopManager.cs    Item setup and purchase flow
│   ├── ShopItemUI.cs     Per-item buy/equip state and persistence
│   ├── PlayerPoints.cs   Point balance and progress reset
│   ├── ScreenShake.cs    Coroutine-based camera shake
│   ├── BGMMusic.cs       Persistent background music
│   └── ...               SFX, score zones, walls, scene loading
├── Art/             Sprites, materials, animations
├── Audio/           Music and SFX
└── Fonts/           Arcade-style TMP font assets
```

## Getting started

**Requirements:** Unity Hub with Unity **6000.0.41f1**

```bash
git clone <repo-url>
```

1. In Unity Hub, choose **Add → Add project from disk** and select the cloned folder.
2. Open `Assets/Scenes/StartMenu.unity` and press **Play**.
3. To build, open **File → Build Profiles**, make sure `StartMenu`, `Gnop` and `Shop` are in the scene list in that order, select Windows or WebGL, and build.

> Scene order matters: `SceneLoader` navigates by build index.

## Third-party assets

- Free Rug Pack and Demo Floor by Azerilo
- Pixel Art Character Package by GDD – Quinnipiac
- Flat Icons [Free] by Heathen Engineering
- ARCADECLASSIC and Karmatic Arcade fonts

All third-party assets remain the property of their respective authors.
