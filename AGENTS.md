# Agent instructions

## Project: "Lichtjes op donker water"
A 3D scrollytelling site (school project, Build: Digital products, Thomas More) by Amel and Hamza about the crossing to Ceuta at the end of July 2026. We tell the story through one boy (Youssef). On our sea every visitor leaves their own light (a "lichtje") behind.

- Goal: the visitor feels the person behind the news figure (scale, closeness, participation).
- Audience: young people aged 16 to 30 in Belgium and the Netherlands.
- Tone: melancholic but hopeful. We do not judge policy.
- Language of the site: Dutch.

### Story flow (one continuous 3D scene, scrolling top to bottom)
1. The sea (start), 2. Zoom in on Youssef, 3. His life, 4. The crossing (night, mist), 5. His outcome and the figures, 6. Your light (choose a dream), 7. Finale (sea full of all visitors' lights, live).

### Stack
- **This repo = frontend**: Vue + Vite + Three.js, deployed on Vercel. Start with `npm start`, build with `npm run build`.
- **Backend is a separate repo**: Node.js + Express + MongoDB Atlas, live updates via websockets (Primus), deployed on Render.
- The frontend talks to the backend with `fetch()`. Never put backend or database code in this repo.

### API contract (do not change without asking)
```
Collections
lichtjes      { droom, afloop, tijd }
hoofdstukken  { volgorde, titel, tekst, beeld, bron }

Endpoints
GET   /hoofdstukken     the story, in order
GET   /lichtjes         latest lights for the sea
POST  /lichtjes         dream in, server picks the outcome, stores it
GET   /lichtjes/stats   count per outcome and per dream

Websocket
lichtje:nieuw           sent to all visitors after every POST
```
- The **server** decides the outcome (arrives, sent back, goes out), never the browser.
- Dreams come from a fixed list: `thuis`, `werk`, `studie`, `familie`, `vrijheid`. No free text input, so nothing hurtful can appear on the sea.

### Vue screens
1. Story (scroll from the sea to the portrait, crossing and outcome)
2. Your light (choose a dream with five buttons, each in the colour of its light)
3. The full sea (closing sentence, counter, sources)

The Three.js scene runs permanently in the background.

### Performance (target 60 FPS, also on mobile)
- Draw all lights in **one `InstancedMesh`** with a simple glow shader, no heavy post-processing.
- Cap visible lights: about 500 on desktop, 200 on mobile.
- On mobile: lighter fog and a lower pixel ratio.
- Compress drawings (WebP) and load them only when their chapter is near.

### Facts and sources
This is about real people. Do not invent facts, figures or quotes. Every figure shown on the site must have its source (NOS, El País). Use only the figures from the concept: 50,000 to 60,000 people, more than 48,300 returned, 67 dead according to the Spanish authorities (the number in Moroccan waters is unknown).

### Code structure
- Source in `src/`. One class or component per file, classes in PascalCase (`Sea.js`, `Light.js`), components as `.vue`.
- Keep the Three.js scene code separate from the Vue components.

## Commits: Conventional Commits
Only commit when the student asks you to. When you do, the commit message MUST use the Conventional Commits format: `<type>(optional scope): <description>`

- Types: `feat`, `fix`, `docs`, `style`, `refactor`, `perf`, `test`, `build`, `ci`, `chore`
- Description in the imperative mood, lowercase, no trailing period.
- Breaking changes: add `!` after the type (`feat!: ...`) or a `BREAKING CHANGE:` footer.
- Examples:
  - `feat(ocean): add animated water surface`
  - `fix(camera): correct aspect ratio on resize`
  - `chore: add vite and three dependencies`

## Shaders: use TSL
- Write shaders with **TSL (Three Shading Language)** from three.js, not raw GLSL strings or `ShaderMaterial`.
- Use node materials (e.g. `MeshStandardNodeMaterial`) and import from `three/tsl`.
- TSL works with `WebGPURenderer` (`three/webgpu`).

## Working together (two people)
- Do not work directly on `main`. Make one branch per issue (e.g. `feat/12-sea-lights`) and merge via a pull request.
- Run `git pull` before you start working, and again before you push.
- Never force push. Never rewrite shared history.

## Security: secrets and credentials
Never commit secrets. This includes:
- API keys, tokens, passwords, private keys, certificates, connection strings
- `.env` files and any file with credentials

Rules:
- Keep secrets in `.env` (or `.env.local`) and make sure these are listed in `.gitignore`.
- Provide a `.env.example` with placeholder values only.
- Read values through environment variables (in Vite: `import.meta.env.VITE_*`). Note that `VITE_*` values end up in the browser bundle, so never put real secrets there.
- Check `git status` and `git diff --staged` before every commit.
- If a secret was committed by accident, tell the user right away so it can be revoked and rotated. Removing it from a later commit is not enough.

## Context: you are working with beginner students
The user is a beginner learning OOP and an AI-assisted workflow. Explain what you change and why, in plain language. Prefer small steps over large rewrites. Only change files that the issue is about.

## Issues first
- Never start coding without an issue number. If the student did not give one, ask for it. If no issue exists yet, ask the student to create one with the issue template on GitHub. Do not create issues for them.
- Every commit references the issue: `Refs #N` while working on it, `Fixes #N` when the issue is done.

## Solving an issue
When the student says "solve issue #N" (or similar):
1. Read the issue (use `gh issue view N` if logged in, otherwise ask the student to paste it). Summarise the goal and the checklist back in one or two sentences.
2. If anything is unclear, ask questions first. Do not guess.
3. Create a branch from an up-to-date `main`: `git pull`, then `feat/N-short-name` (or `fix/N-short-name`).
4. Work in small steps. Only touch the files the issue is about. Explain each step in plain language.
5. Commit in small steps with `Refs #N`. Do not add features that are not in the issue.
6. Check the "definition of done" below and tick off the issue checklist together with the student.
7. Open a pull request. Closing the issue (`Fixes #N` plus the student's reflection) is done as described below.

## Definition of done
- `npm run build` succeeds.
- It works in the browser, also in a narrow mobile-sized window, and the console shows no errors or warnings caused by our code.
- Every item on the issue checklist is met.
- No secrets, no leftover `console.log` or commented-out code.

## Code style
- JavaScript (ES modules), no TypeScript for now. Vue components use `<script setup>` (Composition API).
- Code, variable names and commit messages in English. Domain words stay Dutch (`lichtje`, `droom`, `afloop`). Comments and all text shown on the site in Dutch.
- Keep functions small and name things clearly; a beginner must be able to read it.

## Dependencies
Do not install or add a package (npm) without asking the student first. Say what it is for and why the current packages are not enough.

## Three.js in Vue
- Create the scene once, and keep it separate from the Vue components (see code structure).
- Clean up when a component is removed: call `dispose()` on geometries, materials and textures, stop the animation loop, and remove event listeners (including `resize`) and websocket listeners.
- Do not create objects inside the render loop. Reuse them.

## Files and configuration
- Drawings and other static assets go in `public/`, as compressed WebP. Load them only when their chapter is near.
- The backend URL comes from `.env` as `VITE_API_URL` (never hardcode it). Provide the key in `.env.example` without a real value.

## Reflection when closing an issue
When the student wants to close an issue (a commit with `Fixes #N`), you must NOT write the reflection yourself. Instead:
1. Ask the student these four questions, one at a time, and wait for each answer:
   - Geleerd: which concept do you understand now that you did not understand before?
   - AI hielp: where did the AI really help you forward?
   - Bijgestuurd: where was the AI wrong, or where did you have to adjust your prompt? How did you notice?
   - Volgende keer: what will you do differently next time?
2. If an answer is vague (e.g. "alles", "niks", "het werkte"), ask one follow-up question to make it concrete.
3. Put the student's own answers in the commit body, word for word (only fix spelling). Use the last commit on the branch, or the squash commit message when merging the pull request, in this format:

```
feat: add Line class

Fixes #3

Geleerd: ...
AI hielp: ...
Bijgestuurd: ...
Volgende keer: ...
```
