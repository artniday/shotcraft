# ShotCraft asset library

Runtime artwork for `index.html`. Every file here is loaded by the application; there
are no unreferenced spares. Adding a file does nothing on its own — it also has to be
registered in `index.html` (see below).

## Layout

`3d/` holds 147 PNGs with transparent backgrounds, grouped by category:

| Folder | Contents |
| --- | --- |
| `3d/characters/` | full-body cast art, plus `faces/` and `emirati/faces/` portrait crops and `animals/` |
| `3d/furniture/` | furniture, dressing, vehicles, hand props |
| `3d/production/` | cameras, movement, monitoring, sound, grip |
| `3d/lighting/` | film lights, modifiers, live-event staging |
| `3d/architecture/` | architectural, kitchen, bathroom, exterior |
| `3d/props-extra/` | expanded props and vehicles |
| `3d/hand-stunt/` | hand props and supervised action markers |
| `3d/live-event/` | broadcast, concert, staging, audio |

`environments/` holds 27 JPG backplates for the viewfinder preview, split into
`interiors/` and `exteriors/` with a few at the top level.

## How artwork is wired up

Two different mechanisms, which is worth knowing before you move a file:

- **Props and equipment** are mapped in the `ASSET_3D` object in `index.html`, from a
  tool id to a path fragment such as `"furniture/sofa-3"`. A PNG with no entry in that
  map is never loaded.
- **Cast** is listed in `ACTOR_PROFILES`, which carries a `file` (full body) and a
  `face` (portrait crop) per character.

Default cast paths are built by `embedded3dPath()`, which strips any query string, so
those entries cannot be cache-busted. Emirati and animal entries use literal paths and
do carry a `?v=` string. Replacing one of those files means bumping its version string
in `index.html` too.

## Framing rule for portrait crops

The round portraits in the cast picker are framed on the **face**, not on the alpha
bounding box of the figure — a ghutra or shayla inflates the bounding box, which
shrinks the face and lifts it in the circle.

Target for every crop in `characters/faces/` and `characters/emirati/faces/`:

- square canvas, 512 × 512
- face box height **42%** of image height
- face box centre **39%** from the top

`characters/faces/` predates this rule and still varies between roughly 37% and 46%
face height; `characters/emirati/faces/` is normalised to it.

## Framing rule for animals

Animals render with `object-fit: contain` rather than `cover`, so the whole canvas is
fitted into the circle and any dead space in the PNG becomes visible off-centring.
Each animal is therefore stored on a square canvas with its alpha bounding box exactly
centred.

The canvas is also sized so no part of the animal is cut by the round mask: the
furthest subject pixel from the centre sits at 97% of the circle radius. A tail or ear
that sticks out diagonally is what usually pushes past the edge, so this is measured on
the actual pixels, not the bounding box. Wider animals end up drawn slightly smaller
than narrow ones — that is the circle's geometry, not an inconsistency.

## Dimensions

Real-world sizes live in the `ACTOR_PROFILES` and asset tables in `index.html`, in
metres. They are planning defaults and should be checked against the actual item a
production uses.
