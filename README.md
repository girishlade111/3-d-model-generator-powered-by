# 3D Model Generator — powered by Rodin

Turn text prompts and images into 3D models right in the browser. This app talks to the
**Rodin generative 3D API** (Deemos Hyperhuman) to submit generation jobs, polls their
status, and renders the resulting model in an interactive 3D viewer with full orbit/zoom
controls.

> Originally scaffolded with [v0.app](https://v0.app) and built out into a complete
> generation studio.

## What it does

- **Text-to-3D and Image-to-3D** — submit a text prompt or upload a reference image and
  generate a 3D model via the Rodin API.
- **Job status tracking** — real-time progress bar, status indicator, and loading
  placeholders while the model is being generated.
- **Interactive 3D preview** — view the finished model with a Three.js-powered viewer
  (rotate, zoom, pan) built on `@react-three/fiber` and `@react-three/drei`.
- **Download models** — export the generated model files through a proxied download
  endpoint.
- **Polished UI** — dark, full-screen studio layout with shadcn/ui components, form
  validation (react-hook-form + zod), toasts, and theming via `next-themes`.

## Tech stack

| Layer | Technology |
|---|---|
| Framework | Next.js 15 (App Router), React 19, TypeScript |
| 3D rendering | Three.js, `@react-three/fiber`, `@react-three/drei` |
| UI | Tailwind CSS, shadcn/ui, Radix UI, Lucide icons |
| Forms | react-hook-form, Zod |
| 3D generation | Rodin API (text-to-3D / image-to-3D) |
| Tooling | pnpm |

## Quick start

```bash
# install dependencies
pnpm install

# run the dev server
pnpm dev
```

Open http://localhost:3000 in your browser.

Production build:

```bash
pnpm build
pnpm start
```

## Environment variables

The app calls the Rodin API from server-side API routes, so your Rodin API key must be
configured in the environment. Check `app/api/rodin/route.ts` for the exact variable
name and format expected.

| Variable | Required | Description |
|---|---|---|
| Rodin API key (see `app/api/rodin/route.ts`) | Yes | API key for submitting text-to-3D / image-to-3D jobs |

## Project structure

```
app/
  api/
    rodin/          # POST: submit a generation job to the Rodin API
    status/         # POST: poll generation job status
    download/       # POST: fetch the finished model file
    proxy-download/ # proxied model download for the client
  page.tsx          # full-screen entry → <Rodin /> studio
  layout.tsx        # root layout, theme provider, global CSS
components/
  rodin.tsx         # main generation studio (form + viewer orchestration)
  model-viewer.tsx  # Three.js interactive 3D viewer
  form.tsx          # generation form (prompt / image upload / options)
  options-dialog.tsx, progress-bar.tsx, status-indicator.tsx, ...
  ui/               # shadcn/ui primitives
lib/
  api-service.ts    # client wrappers for /api/rodin, /api/status, /api/download
  form-schema.ts    # zod validation schema for the generation form
hooks/
  use-media-query.ts
public/             # static assets / placeholders
styles/            # global styles
```

## API routes

- `POST /api/rodin` — submit a generation job (FormData: prompt, image, options)
- `POST /api/status` — `{ "subscription_key": "..." }` → job status + progress
- `POST /api/download` — `{ "task_uuid": "..." }` → download links for the model

## Deployment notes

This is a **dynamic Next.js app** — the `/api/*` routes run server-side and proxy a
secret API key, so it cannot be statically exported. Deploy it to a Node-capable host
(Vercel, Netlify, or any server running `next start`). Make sure the Rodin API key env
variable is set on the hosting provider, and set `NEXT_PUBLIC_*` vars if any are
introduced later.

## License

Free to use and modify.

---

Built by Girish Lade · https://ladestack.in
