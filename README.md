# Ponguelike

A juiced-up take on classic **Pong**, built in Unity 6, with a light roguelike-style progression loop: beat the AI, earn points, and spend them in the shop on cosmetics and upgrades.

## Features

- **Two game modes**: local Player vs Player, or Player vs AI
- **Beatable AI**: tracks the ball with a dead zone and randomly varying speed
- **Game feel**: the ball speeds up on every paddle hit, plus screen shake, particles, a ball trail and sound effects
- **Persistent points**: earned in Player vs AI matches and saved between sessions
- **Shop**: buy and equip skins and upgrades with your points

## Controls

| Player | Up | Down |
|---|---|---|
| Player 1 (left) | `W` | `S` |
| Player 2 (right) | `↑` | `↓` |

The first player to score **3** wins the match.

## Progression

Points are only awarded in **Player vs AI** mode:

| Event | Points |
|---|---|
| You score a goal | +1 |
| You win the match | +3 |
| You lose the match | +1 |

### Shop items

| Item | Cost | Effect |
|---|---|---|
| Geometric reference | 1 | Replaces the ball with a cube skin |
| The First Paddle | 1 | Alternative sprite for Player 1's paddle |
| GrowthSpurt | 5 | Player 1's paddle becomes 1.5× longer |

Progress is stored in `PlayerPrefs`. The shop has a reset button that clears your points and purchases.

## Getting started

### Requirements

- **Unity 6000.0.41f1** (Unity 6). Other Unity 6 versions will likely work too.

### Running the project

1. Clone the repository:
   ```bash
   git clone <repo-url>
   ```
2. Open the folder in **Unity Hub** (*Add → Add project from disk*).
3. Open `Assets/Scenes/StartMenu.unity` and press **Play**.

### Building

The project targets **Windows** and **WebGL**. Open *File → Build Profiles*, make sure `StartMenu`, `Gnop` and `Shop` are included, then build for your platform.

## Project structure

```
Assets/
├── Scenes/
│   ├── StartMenu.unity   # Entry point
│   ├── Gnop.unity        # The Pong match
│   └── Shop.unity        # Spend points on items
├── Scripts/
│   ├── GameManager.cs    # Score, win checks, game modes, point rewards
│   ├── Ball.cs           # Ball movement, speed-up, collisions, skins
│   ├── Paddle.cs         # Player input, AI movement, paddle skins/upgrades
│   ├── GameUI.cs         # Menus, scoreboard, end screens
│   ├── ShopManager.cs    # Shop setup and purchase handling
│   ├── ShopItemUI.cs     # Buy / equip state for each item
│   ├── PlayerPoints.cs   # Saved point balance
│   ├── ScreenShake.cs    # Camera shake
│   └── ...               # Audio, scene loading, walls, score zones
├── Art/                  # Sprites, materials, animations
├── Audio/                # Music and SFX
└── Fonts/                # Arcade-style fonts
```

## Built with

- [Unity 6](https://unity.com/) (2D)
- TextMesh Pro
- Unity Post Processing

## Credits

Third-party assets from the Unity Asset Store:

- **Free Rug Pack** and **Demo Floor** by Azerilo
- **Pixel Art Character Package** by GDD – Quinnipiac
- **Flat Icons [Free]** by Heathen Engineering
- **ARCADECLASSIC** and **Karmatic Arcade** fonts

All rights to these assets belong to their respective authors.
