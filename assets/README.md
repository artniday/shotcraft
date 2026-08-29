# ShotCraft asset library

This folder is the stable home for reusable visual assets. `catalog.json` records real-world dimensions in metres and `shotcraft-assets.svg` provides lightweight SVG symbols that can be referenced with `<use href="assets/shotcraft-assets.svg#asset-id">`.

Categories include characters, furniture, vegetation, cameras, lighting, grip, architecture, vehicles and production equipment. Keep `shotcraft-characters-cutout.png` in the repository root for compatibility with existing projects; new files may be added here and registered in `catalog.json`.

## Generated 3D library

`3d/` contains eight master contact sheets and 118 individually addressable PNG assets, plus the master sheets:

- `3d/furniture/` — furniture, dressing, vehicles and hand props
- `3d/production/` — cameras, movement, monitoring, sound and grip
- `3d/lighting/` — film lights, modifiers and live-event staging
- `3d/architecture/` — architectural, kitchen, bathroom and exterior assets
- `3d/props-extra/` and `3d/hand-stunt/` — expanded props, vehicles and supervised action markers
- `3d/live-event/` — broadcast, concert, staging and audio systems
- `3d/characters/` — adult man, adult woman, baby, senior man, senior woman and child

The ShotCraft palette uses these files automatically when a matching tool item is available. The master sheets are retained as the source record for future recropping or higher-resolution exports.

All dimensions are planning defaults and should be verified against the actual item used by the production.
