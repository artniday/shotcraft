# ShotCraft

A single-file browser tool for shot design and floor plans, built for the Applied Media
Division at Higher Colleges of Technology. Plan a set from above, place cast and
equipment, check what falls inside the frame, and export a print pack.

No build step and no dependencies. `index.html` is the whole application; everything
else is artwork it loads at runtime.

## Running it

Open `index.html` in a browser, or serve the folder:

```bash
python3 -m http.server 8000
```

Then visit <http://localhost:8000>.

Opening the file directly with `file://` works in most browsers, but some block
local image loads. If artwork does not appear, use the server command above.

## Publishing to GitHub Pages

Push the repository contents to the branch you publish from, then set
**Settings → Pages → Build and deployment** to *Deploy from a branch*, and choose
that branch with the `/ (root)` folder. `index.html` and `assets/` must both sit at
the root of what you publish. The `.nojekyll` file keeps Pages from running the
files through Jekyll.

## Layout

```
index.html                      the application
assets/3d/characters/           cast: default, Emirati, animals, plus circular face crops
assets/3d/furniture/            furniture, dressing, vehicles, hand props
assets/3d/production/           cameras, movement, monitoring, sound, grip
assets/3d/lighting/             film lights, modifiers, live-event staging
assets/3d/architecture/         architectural, kitchen, bathroom, exterior
assets/3d/props-extra/          expanded props and vehicles
assets/3d/hand-stunt/           hand props and supervised action markers
assets/3d/live-event/           broadcast, concert, staging, audio
assets/environments/            backplates for the viewfinder preview
```

## Drawing a plan

The floor plan behaves like a home design tool rather than a drawing canvas:

- An empty scene offers **Draw a 5 x 4 m room**, which lays four joined walls at once.
- Dragging furniture near a wall makes it sit **flush against that wall** and turn its
  back to it. Dragging it near another object **lines the two up**, and the guide lines
  show which edges or centres matched.
- Drawing a wall **rounds to a clean bearing** when it is within a few degrees of one,
  and an endpoint dropped near an existing corner **welds onto it**, so rooms close.
- The **length and angle** of a wall are shown while you draw it.

All of that is controlled by the **Snap & guides** button in the View menu. Turn it
off and every position stays exactly where you put it.

## Timeline, keyframes and live cutting (stage 1 merge)

The beat bar is now a timeline in seconds. Each actor, camera and prop has a row of
keyframe diamonds. Move the playhead, drag the element, and a keyframe is recorded
(AUTO-KEY). Drag the orange square on a path to bend it, double-click a path to add a
keyframe, right-click a diamond to delete it. Beats are markers on the ruler and the
storyboard still uses them.

The switcher above the plan cuts between cameras at the playhead (click, or keys 1-9).
Cuts land in the red CUTS track; the live camera shows a LIVE badge and the viewfinder
can follow the cuts. Older projects and templates migrate their beats to keyframes on
first load.

## Render (stage 2 merge)

**Render** in the top bar plays the scene once and saves it: the floor plan animation,
the viewfinder following the cuts (the edit), one camera, or both side by side. Output is
a video (MP4 where the browser can encode it, otherwise WebM, recorded in real time at
30 fps), a PNG image sequence in a zip, or a single PNG of the current frame. Figures
that travel between keyframes bob and sway in the viewfinder like a walk cycle.

`assets/inline/` holds a compact WebP copy of every picture wrapped in a script. It
exists only so the renderer can read the pictures when `index.html` is opened straight
from disk (`file://`), where browsers block `fetch()`. Leave it out when uploading to a
web host; regenerate it after changing artwork.

## Visibility and lighting simulation (stage 2.1)

This showcase build increases plan/UI contrast so objects, labels, grid lines and controls read more clearly on screen.

The viewfinder now uses each placed fixture's **aim, beam angle, wattage, dimmer, height and Kelvin value** to simulate visible light pools and colour on the environment and characters. This is an educational previsualisation effect rather than a physically based renderer, but it makes key, fill, rim, practical and background lighting choices much easier to demonstrate.

## Language

The **عربي / EN** button in the top bar and on the launch screen switches the whole
interface between English and Arabic (right-to-left). Add phrases to `I18N_AR` in
`index.html` to extend the translation.

## Working on the artwork

Character artwork comes in two forms. The full-body PNG (`characters/<name>.png`)
is drawn on the plan; the face crop (`characters/faces/<name>.png`) fills the round
portrait in the cast picker.

Face crops are framed on the **face**, not on the bounding box of the figure. Headwear
inflates a bounding box, so a tight crop pushes the face smaller and higher in the
circle and the set stops looking consistent. The current target across every portrait
is a face box **42% of the image height** with its centre **39% from the top**. Match
that when adding or replacing a face crop.

Assets referenced with a `?v=` query string are cache-busted. If you replace one of
those files, bump the string everywhere it appears in `index.html`, or browsers will
keep serving the old artwork.

## Licence

No licence has been declared. All rights reserved by the author unless stated
otherwise; the third-party artwork in `assets/` is subject to its own terms.
