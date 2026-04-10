# Promptify

Promptify is a prompt marketplace and library with coin-based unlocks, ratings, reviews, and an admin dashboard. The frontend is a Vite + React app, and the backend is Supabase (Postgres + Auth + RLS + RPC).

## Tech Stack

<p align="center">
  <a href="https://skillicons.dev">
    <img src="https://skillicons.dev/icons?i=js,ts,react,vite,css" />
  </a>
</p>

- React 19 and Vite
- TypeScript
- Tailwind CSS
- Supabase (Auth, Postgres, RLS, RPC)
- Vitest and Playwright for testing

## Features

- Browse prompts with search, filters, and trending sorting
- Unlock prompt text with coins and track unlocks
- Rate and review prompts
- Profile page with coins and unlocked items
- Admin dashboard for managing prompts
- Auth via Google OAuth and email (password or magic link)

## Screenshots

Replace these placeholders with real images.

<a href="https://postimg.cc/bZ0rZ6kk" target="_blank"><img src="https://i.postimg.cc/bZ0rZ6kk/Screenshot-20260131-170620-Chrome.jpg" alt="Screenshot-20260131-170620-Chrome"></a> <a href="https://postimg.cc/zywvydK5" target="_blank"><img src="https://i.postimg.cc/zywvydK5/Screenshot-20260131-170628-Chrome.jpg" alt="Screenshot-20260131-170628-Chrome"></a> <a href="https://postimg.cc/4YRnNCKj" target="_blank"><img src="https://i.postimg.cc/4YRnNCKj/Screenshot-20260131-170631-Chrome.jpg" alt="Screenshot-20260131-170631-Chrome"></a> <a href="https://postimg.cc/YvN0vXYB" target="_blank"><img src="https://i.postimg.cc/YvN0vXYB/Screenshot-20260131-170635-Chrome.jpg" alt="Screenshot-20260131-170635-Chrome"></a><br><br>
<a href="https://postimg.cc/4YRnNCKr" target="_blank"><img src="https://i.postimg.cc/4YRnNCKr/Screenshot-20260131-170640-Chrome.jpg" alt="Screenshot-20260131-170640-Chrome"></a> <a href="https://postimg.cc/9DHzF3Rh" target="_blank"><img src="https://i.postimg.cc/9DHzF3Rh/Screenshot-20260131-170643-Chrome.jpg" alt="Screenshot-20260131-170643-Chrome"></a> <a href="https://postimg.cc/87gsP8Jk" target="_blank"><img src="https://i.postimg.cc/87gsP8Jk/Screenshot-20260131-170656-Chrome.jpg" alt="Screenshot-20260131-170656-Chrome"></a> <a href="https://postimg.cc/3kQWJMyC" target="_blank"><img src="https://i.postimg.cc/3kQWJMyC/Screenshot-20260131-170659-Chrome.jpg" alt="Screenshot-20260131-170659-Chrome"></a><br><br>

## Local Setup (Frontend)

1. Install dependencies.

```bash
npm install
```

2. Create a `.env` file and add your Supabase API values.

```bash
VITE_SUPABASE_URL=https://<project-ref>.supabase.co
VITE_SUPABASE_ANON_KEY=<your-anon-key>
```

3. Run the dev server.

```bash
npm run dev
```

The app will be available at `http://localhost:3000`.

## Backend Setup (Supabase)

The SQL database schema is stored in Git as `supabase_schema.sql`. You must apply it using the Supabase SQL Editor.

1. Create a new Supabase project.
2. Open the Supabase dashboard and go to SQL Editor.
3. Copy the contents of `supabase_schema.sql` and run it.
4. Confirm the tables, RLS policies, and RPC functions were created.

Important pieces created by the schema:

- Tables: `profiles`, `prompts`, `prompt_secrets`, `unlocks`, `reviews`
- RLS policies for public reads and secure writes
- RPC functions: `unlock_prompt`, `update_prompt_rating`
- Trigger: `handle_new_user` to auto-create profiles

### Create an Admin User (Optional)

If you need access to the Admin Dashboard, set your profile role to `admin`:

```sql
update profiles set role = 'admin' where email = 'you@example.com';
```

## Auth Providers (Supabase)

This app supports Google OAuth and email (password or magic link).

### Provider Registration Data

Fill in the values below in Supabase Auth Providers.

Google OAuth

- Client ID: <your-google-client-id>
- Client Secret: <your-google-client-secret>
- Authorized redirect URI: https://<project-ref>.supabase.co/auth/v1/callback

Email

- Provider enabled: yes
- SMTP: optional (configure if you want custom email sender/domain)

### Redirect URL Configuration

In Supabase go to Authentication -> URL Configuration.

Set the Site URL and allowed redirect URLs so OAuth and magic links return to your app:

Local development

- Site URL: http://localhost:3000
- Additional Redirect URLs: http://localhost:3000

Production (Cloudflare Pages)

- Site URL: https://<your-cloudflare-pages-project>.pages.dev
- Additional Redirect URLs: https://<your-cloudflare-pages-project>.pages.dev

Note: The app uses `window.location.origin` for both OAuth and magic link redirects, so the origin must be in the allowed list.

## Environment Variables

Set these in `.env` for local development and in Vercel for production.

```bash
VITE_SUPABASE_URL=https://<project-ref>.supabase.co
VITE_SUPABASE_ANON_KEY=<your-anon-key>
```

You can find these in Supabase: Project Settings -> API.

## Deployment (Cloudflare Pages)

1. Push the repo to GitHub.
2. Go to Cloudflare Dashboard -> Pages.
3. Create a new project and connect your GitHub repo.
4. Build settings:
   - Build command: `npm run build`
   - Build output directory: `dist`
5. Add environment variables in Pages -> Settings -> Environment variables:
   - `VITE_SUPABASE_URL`
   - `VITE_SUPABASE_ANON_KEY`
6. Deploy.

After the first deploy:

- Update Supabase Site URL and Redirect URLs to the Cloudflare Pages domain (e.g., `https://your-project.pages.dev`).
- Keep the Google OAuth redirect URI set to the Supabase callback URL: `https://<project-ref>.supabase.co/auth/v1/callback`.

### Alternative: Deploy with Wrangler CLI

If you prefer CLI deployment:

1. Install Wrangler: `npm install -g wrangler`
2. Login: `wrangler auth login`
3. Deploy: `wrangler pages deploy dist`

## Scripts

- `npm run dev` - start Vite dev server
- `npm run build` - production build
- `npm run preview` - preview the production build
- `npm run lint` - lint
- `npm run format` - format code with Prettier
- `npm run format:check` - check formatting
- `npm run test` - run unit and integration tests
- `npm run test:watch` - watch mode for tests
- `npm run test:e2e` - Playwright end-to-end tests
- `npm run typecheck` - TypeScript typecheck

## Troubleshooting

- If you see "Supabase is not configured", make sure `.env` has `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` and restart the dev server.
- If Google sign-in fails, verify the provider is enabled and the redirect URI matches the Supabase callback URL.
- If magic links do not open the app, verify the Site URL and Additional Redirect URLs in Supabase.
