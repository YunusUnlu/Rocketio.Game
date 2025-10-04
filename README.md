Rocketio.Game — Space Devourer
A tiny .io-style browser arcade. Eat smaller celestial objects to grow, avoid the big ones, pick up ammo and health, and clear the map!
Quick start: open index.html in a modern browser. (Local server recommended—see below.)
Eat & grow: Your ship eats smaller objects and grows in size.

Directional ship: Ship rotates toward last movement; a flame effect intensifies on boost.

Dynamic camera: Automatic zoom-out as you grow, keeping on-screen size comfortable.
Minimap: Marks targets as edible / dangerous / neutral in real time.
Shooting: Planets are destroyed after two hits.
Pickups: Ammo (bullets) and Health (extra life) spawns.
HUD: Lives, timer, score, size, boost bar, ammo.
Score history: Stores last games and best time via localStorage.
Edge pickup fix: Easier collecting/eating when items sit near world boundaries.

How to Play
Goal: Consume smaller bodies and destroy larger ones until the field is clear.
Rule: You can only eat objects smaller than you. Significantly larger ones damage you.
Shooting: Two bullet hits destroy a planet.
Pickups: Gold boxes = ammo, red orbs = health.
End: Clear all objects → Victory. Lose all lives → Game Over.

Controls
Move: WASD / Arrow Keys
Boost: Shift
Shoot: Space
Restart: R
Start: “LETS GO!” button

Run Locally
Clone the repo or download index.html.
Open in a browser or run a simple local server:
Python 3: python -m http.server 8080

Node (http-server): npx http-server .
Navigate to the served URL and play.
GitHub Pages: Keep index.html in repo root → Settings → Pages → “Deploy from a branch”.

Project Structure
Single-file build:
.
└── index.html   # HTML + CSS + JS in one file

Technical Notes
Game Loop
requestAnimationFrame drives gameLoop (movement, bullets, collisions, timer).

Camera & Smart Zoom
Computes a scale based on targetShipScreen / shipSize, clamped to [minScale, maxScale].
Smoothly lerps to the target scale to avoid camera jitter.
Keeps the ship centered while zooming.

Directional Ship & Flame
Last movement vector → atan2 → rotation in degrees.
Flame element scales/opacity up when boosting; hidden when idle.
Collision & Edge Tolerance
Circular collision via distance check.
When near walls, a small slack tolerance helps collect/eat items stuck at edges.

Shooting
Bullet velocity follows lastDirection.
On hit, a planet accumulates hits; at >= 2 it’s consumed.

Minimap
World → minimap coordinates normalized by world size.
Status color updates with your current size (edible / dangerous / neutral).

Score & History
localStorage keeps the last 5 games and best completion time; best run is highlighted.

Customization (Quick Knobs)
Inside SpaceDevourerGame, tweak these:

// Camera & Zoom
this.targetShipScreen = 40;   // Desired on-screen ship size in px (smaller → more zoom-out sooner)
this.minScale = 0.35;         // Max zoom-out
this.maxScale = 2.0;          // Max zoom-in
this.zoomSmooth = 0.08;       // 0.05–0.15 feels good

// World
this.worldWidth = 2400;
this.worldHeight = 2400;

// Ship
this.spaceshipSize = 30;      // Starting size
this.boostEnergy = 50;
this.maxBoostEnergy = 50;

// Ammo
this.ammo = 5;
this.maxAmmo = 5;

// Object distribution (createCelestialObjects)
{ class:'asteroid', size:[10,20], count:25, points:5 }
// ...others

// Growth rate (consumeObject)
this.spaceshipSize += obj.size * 0.25; // lower to slow growth

// Edge tolerance (checkCollisions)
const slack = nearWall ? 8 : 0; // increase a bit if edge pickups still feel tricky


Tips:
Want a smaller ship on screen as you grow? Reduce targetShipScreen to ~32.
Prefer softer camera? Lower zoomSmooth (0.06); snappier? Raise it (0.12).

Tips & Performance
On low-end machines:
Reduce object/pickup counts.
Simplify CSS shadows/blur.
Shorten transition durations for UI elements.
Contributing

PRs welcome!
Fork → feature branch → make changes.
Use clear commit messages.

Open a PR describing what you changed and why.

License

MIT — free to use, modify, and distribute. Attribution appreciated. 
Enjoy—and may your ship devour the cosmos! 
