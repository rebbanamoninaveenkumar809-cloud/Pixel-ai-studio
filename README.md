# PIXEL AI STUDIO
React + Vite + Tailwind. Free to run and host (Vercel, Netlify, Cloudflare Pages).

## Run
npm install && cp .env.example .env && npm run dev

## What works with no setup
Landing, routing, canvas editor (text, fonts, colors, image upload, layers, drag, undo/redo, filters, PNG download), local saved projects, download history.

## Supabase (optional)
Add VITE_SUPABASE_URL and VITE_SUPABASE_ANON_KEY (public anon key only). Run in the SQL editor:

    create table projects(id bigint generated always as identity primary key,
      user_id uuid not null default auth.uid() references auth.users,
      name text, preset text, data jsonb, created_at timestamptz default now());
    alter table projects enable row level security;
    create policy "own rows" on projects for all using (auth.uid()=user_id) with check (auth.uid()=user_id);

Google login: enable the provider in Supabase > Authentication.

## AI integration (not connected by default)
Set VITE_AI_ENDPOINT to a serverless function you host (Supabase Edge Function, Cloudflare Worker, Vercel function). It keeps the provider API key as a server secret.
Contract: POST {prompt,preset,width,height} -> {image: "<https url or data: URL>"}. Add similar functions for background removal and video, then wire buttons in Editor.jsx.
Video editing and text generation are placeholders (src/components/Pages.jsx > Pending). ffmpeg.wasm is the free route for trimming/resizing video.

## Payments
Not implemented. Pro and Premium buttons only show a notice. Plans and credit limits live in src/lib/data.js.
