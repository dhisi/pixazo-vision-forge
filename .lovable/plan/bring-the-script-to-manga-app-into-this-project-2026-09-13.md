# Bring the "Script to Manga" app into this project

Copy the full working app from the GitHub repository into this project, wire up the ten image keys and the text key as secure server-side secrets, and verify a real run end to end.

## What the app does (from the repository)

- One mobile-friendly page where you paste a timestamped script.
- The text service (Agnes AI, using the model already selected in the code) turns each scene into an image prompt.
- The image service (Pixazo) renders the panels, spreading work across all ten keys in parallel with retries and rate-limit handling.
- Progress is saved in the browser so a long run survives a reload, and finished panels can be exported as an MP4 video.
- A stop control cancels a run in progress.

## Steps

1. Copy the app across: the page, all its logic (script parsing, prompt generation, image generation, key pool, progress saving, video export), the API endpoints, the shared styles, and the UI components. The project's existing placeholder home page is replaced by the app's page.
2. Match the repository's package list so image/video export and everything else works, then install.
3. Save all eleven keys in the encrypted secret store — ten image keys and one text key. They stay server-side only: no key is written into any project file and none reaches the browser. Image requests are proxied through the server so keys never appear in browser traffic.
4. Add proper page title and description for the app.
5. Verify: build and typecheck clean, then run the page in a real browser, submit a short script, confirm prompts come back from the text service and real images come back from the image service, check the stop control, and confirm no key appears in browser network traffic or page source.

## Technical notes

- Same stack as this project (TanStack Start + Tailwind v4), so files transfer directly; no database or login is needed.
- Secret names the code reads: `PIXAZO_API_KEY` plus `PIXAZO_API_KEY_2` … `PIXAZO_API_KEY_10`, and `AGNES_API_KEY`. `AGNES_MODEL` is left unset so the code's already-selected free model is used.
- Keys are read only inside server handlers (`src/lib/keys.server.ts`, `src/lib/agnes.server.ts`); nothing is exposed via `VITE_` variables.
- The keys pasted in chat will be stored through the secret store and not echoed back or committed. Since they were shared in a public chat, rotating them at the providers later is advisable.
