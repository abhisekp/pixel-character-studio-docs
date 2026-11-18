# Game Design Document

## Product and audience

Pixel Character Studio is a game-asset authoring tool, not a playable game. Its users are independent game developers and pixel artists producing characters for top-down and side-view games. Its core promise is an editable local asset pipeline with explicit generation costs, durable files and reference identity.

## Design pillars

1. Characters remain editable as pixels and portable PNG files.
2. Offline creation works without a cloud account; BYOK providers are optional.
3. Reference identity takes precedence over animation variety.
4. Every generation is deliberate, cancellable and reviewable.
5. Project changes are undoable; existing files are not silently overwritten.

## Core production loop

Describe or import a character → choose size and camera → generate/draw one direction → review silhouette and colors → refine pixels → export directions or a reference-locked idle animation → import into a game engine.

## Character specification

Projects store a name, description, character type, camera, outline/detail guidance, canvas size, and independent directional pixel frames. Supported sizes are 32, 48, 56, 64 and 128. Four-direction and eight-direction layouts preserve hidden diagonal frames. Pixelorama projects and JSON editor projects are separate formats.

## Character identity contract

The selected directional frame acts as the immutable reference for idle export. Eight frames translate the complete reference by a bounded vertical offset. Colors, occupied pixel count, proportions, facial features and costume pixels remain identical; a two-pixel transparent border prevents clipping. Output cells are original size plus four pixels, arranged horizontally, 100 ms per frame, looping at 10 FPS. Animation export does not replace directional project frames.

This guarantees pixel identity for this idle motion only. It is not articulated walking or AI video generation. BYOK reference generation sends the reference image to supported image-edit endpoints, but model-produced anatomy and rotations require human review. Text-only Ollama does not accept image references. No claim of guaranteed identity is made for independently generated AI frames.

## Art direction and acceptance

Use readable silhouettes, limited palettes, deliberate outlines, integer pixel coordinates and transparent backgrounds. Review reference identity at native scale and enlarged nearest-neighbor scale. Reject clipped feet, missing accessories, incorrect colors and accidental direction changes. A production export should have a clear name, consistent frame dimensions and a declared frame order.

## Controls and interface

Create Character owns description, provider, camera and generation scope. Pixel Editor owns drawing, fill, erase, sampling, direction selection and asset export. Generation settings discovers installed Ollama models from its API. Loading, unavailable-service and provider errors must be actionable. Updates are explicit; installation and restart are separate from checking.

## Data and security

Local JSON projects, bounded undo history and autosave support recovery. Save a project file before app restart. Cloud API keys stay in native session memory. Generation uses strict drawing data and bounded images. Pixelorama MCP is trusted local executable code. The separate editing MCP bridge uses a per-session credential. Release downloads require cryptographic updater signatures; publisher certificate signing is an additional Windows trust layer.

## Progression and monetization

There is no player progression, scoring, combat or in-app payment system. The tool has no subscription enforcement. Cloud generation is billed by the user's provider. Do not add game mechanics or commercial limits without a separate product decision.

## Delivery milestones

- Delivered foundation: local editor, project save/load, directional exports, Pixelorama MCP and explicit provider generation.
- Current release: reference-preserving idle sheet export, signed OTA plumbing, VitePress documentation and release workflows.
- Future: articulated animation with identity evaluation, reference-bible metadata, batch review and engine-specific export presets. These are planned, not implemented.

## Quality gates

Unit tests verify idle frame count, source immutability, palette/occupied-pixel preservation and edge safety. Build and type checks cover native and renderer code. Paid providers require separately authorized live tests. A real update from an installed older version to a later signed release must be tested before claiming OTA installation is verified.
