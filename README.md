# AiAdGenerator

Lightweight web app for generating AI ad assets (images/videos) — React + Vite frontend and Node + Express + Prisma backend.

**Repository layout**

- [client](client) — React + Vite + TypeScript frontend
- [server](server) — Node (TypeScript) API, Prisma, Cloudinary, Google GenAI integration

**Key technologies**

- Frontend: React, Vite, TypeScript, TailwindCSS
- Backend: Node, Express, TypeScript
- Auth: Clerk
- AI: Google GenAI (@google/genai)
- File storage: Cloudinary
- DB: PostgreSQL via Prisma

## Prerequisites

- Node.js (v18+ recommended)
- npm or pnpm
- PostgreSQL-compatible database (local Postgres, Neon, Supabase, etc.)

## Environment variables

Create a `.env` file in the `server` folder (copy values from `server/.env` for reference if present). At minimum set:

- `DATABASE_URL` — Prisma-compatible Postgres connection string
- `GOOGLE_CLOUD_API_KEY` — Google GenAI API key
- `CLOUDINARY_URL` — Cloudinary URL (cloudinary://...)
- `CLERK_PUBLISHABLE_KEY`, `CLERK_SECRET_KEY`, `CLERK_WEBHOOK_SIGNING_SECRET`

See the example values already in [server/.env](server/.env) for reference.

## Database (Prisma + Postgres)

1. Point `DATABASE_URL` at a Postgres instance you control (local Postgres, Neon, Supabase, etc.). Example:

   DATABASE_URL="postgresql://USER:PASSWORD@HOST:PORT/DATABASE?schema=public"

2. From the `server` folder, install dependencies and generate Prisma client:

```bash
cd server
npm install
npx prisma generate
```

3. Apply migrations (if you want to run migrations locally):

```bash
npx prisma migrate dev --name init
```

Note: this project already includes an initial migration under `server/prisma/migrations`.

Schema is at [server/prisma/schema.prisma](server/prisma/schema.prisma).

## Install & Run (local)

1. Install server deps and start API:

```bash
cd server
npm install
npm run server   # uses nodemon + tsx, listens on PORT (default 5000)
```

2. Install and run client:

```bash
cd ../client
npm install
npm run dev      # starts Vite dev server
```

3. Open the frontend URL shown by Vite (usually http://localhost:5173) and ensure backend is reachable at http://localhost:5000.

Optional: to expose your local server publicly the project includes a `tunnel` script in `server/package.json` that uses Cloudflare `cloudflared`:

```bash
cd server
npm run tunnel
```

## Notes on services

- Google GenAI: set `GOOGLE_CLOUD_API_KEY` in `server/.env`. See [server/configs/ai.ts](server/configs/ai.ts).
- Cloudinary: set `CLOUDINARY_URL` in `server/.env`. Files are uploaded from `server/controllers/projectController.ts`.
- Clerk: configure Clerk keys for authentication (see `server/.env`). Frontend depends on `@clerk/clerk-react`.

## For deployment

1. Just do/create a project on Google Cloud - https://cloud.google.com.
2. Then create an API key in Google AI Studio (or the appropriate GenAI console) and enable the required APIs for your project.
3. Take that API key and set `GOOGLE_CLOUD_API_KEY` in `server/.env`.
4. Verify the AI endpoints locally by running the project once and checking generation features.
5. Lastly, deploy the frontend and backend to your preferred hosting provider.

Cloudinary connection steps

1. Create a Cloudinary account at https://cloudinary.com and set up an upload preset if required.
2. From your Cloudinary dashboard get the cloud name, API key, and API secret.
3. Set `CLOUDINARY_URL` in `server/.env` using the format `cloudinary://API_KEY:API_SECRET@CLOUD_NAME` or set the individual env vars the Cloudinary SDK expects.
4. Test uploads locally by creating a small API request that uploads a sample file or by running the app and using the upload flow.

## Useful scripts

- Server: `npm run server` (development), `npm run start` (start once)
- Client: `npm run dev`, `npm run build`, `npm run preview`

## Troubleshooting

- If Prisma cannot connect, confirm `DATABASE_URL` and DB network settings (SSL, allowed IPs).
- If uploads fail, confirm `CLOUDINARY_URL` is valid and Cloudinary account has available resources.

---

If you'd like, I can: add a sample `.env.example`, commit these changes, or add a short one-command setup script. Which would you prefer next?
