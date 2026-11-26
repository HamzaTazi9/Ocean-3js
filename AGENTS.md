# Agent instructions

## Project
Three.js project (Vite). Start with `npm start`, build with `npm run build`.

## Commits: Conventional Commits
Use the Conventional Commits format: `<type>(optional scope): <description>`

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

## Reflection when closing an issue
When the student wants to close an issue (a commit with `Fixes #N`), you must NOT write the reflection yourself. Instead:
1. Ask the student these four questions, one at a time, and wait for each answer:
   - Geleerd: which concept do you understand now that you did not understand before?
   - AI hielp: where did the AI really help you forward?
   - Bijgestuurd: where was the AI wrong, or where did you have to adjust your prompt? How did you notice?
   - Volgende keer: what will you do differently next time?
2. If an answer is vague (e.g. "alles", "niks", "het werkte"), ask one follow-up question to make it concrete.
3. Put the student's own answers in the commit body, word for word (only fix spelling), in this format:

```
feat: add Line class

Fixes #3

Geleerd: ...
AI hielp: ...
Bijgestuurd: ...
Volgende keer: ...
```
