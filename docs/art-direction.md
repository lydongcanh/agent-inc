# Art direction

## Look

- **Cozy toon.** Soft two-band toon shading, soft tinted outlines, lavender-tinted shadows, warm light, and bloom on screens and lamps.
- **Low-poly, rounded shapes.** Beveled, smooth forms colored from a small palette (vertex colors), with no detailed textures. No blocky or voxel style.
- **Light on hardware.** Low polygon counts and simple shaders, so the game runs smoothly on modest machines.

## Characters

- **Chibi workers.** Base body, skeleton and animations come from [KayKit Character Pack: Adventurers](https://github.com/KayKit-Game-Assets/KayKit-Character-Pack-Adventures-1.0) (CC0).
- **Our own parts.** Hairstyles, office outfits and accessories are made in Blender and fitted to the KayKit skeleton.
- **Generated per agent.** Each spawned agent gets a random combination of skin tone, hairstyle, hair color, outfit and accessory.
- **Scale.** Workers are sized to the furniture (a seated worker fits the desk), slightly smaller than full human scale.

## Animations

- KayKit's animations cover idle, walking, sitting, interacting and more. Office-specific actions such as typing are added on top.
- Extra animations can be retargeted onto the same skeleton through Godot's humanoid profile.

## Environment

- Furniture and rooms are built with Blender Python scripts and exported as `.glb`.

## Asset rules

- Every committed third-party asset must allow redistribution (CC0 preferred) and is listed in `game/assets/CREDITS.md`.
- Never commit assets whose license forbids sharing the raw files.
