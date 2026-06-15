# Deploying DoorDesk ERP to Vercel

## 1. Push to GitHub

```bash
cd doordesk
git init
git add .
git commit -m "Initial commit: DoorDesk ERP"
git branch -M main
git remote add origin https://github.com/<your-username>/doordesk-erp.git
git push -u origin main
```

## 2. Set up Supabase (one-time)

1. Create a project at https://supabase.com
2. Go to **SQL Editor** → run `supabase/schema.sql` (creates tables, RLS, triggers)
3. Optionally run `supabase/seed.sql` for sample equipment data
4. Go to **Project Settings → API** and copy:
   - `Project URL`
   - `anon public` key

## 3. Deploy to Vercel

1. Go to https://vercel.com/new and import your GitHub repo
2. Framework preset: **Next.js** (auto-detected)
3. Add Environment Variables:

   | Key | Value |
   |---|---|
   | `NEXT_PUBLIC_SUPABASE_URL` | your Supabase Project URL |
   | `NEXT_PUBLIC_SUPABASE_ANON_KEY` | your Supabase anon key |

4. Click **Deploy**

## Demo Mode (no Supabase yet)

The app builds and runs even **without** Supabase env vars — login/auth is
bypassed and all pages show static demo data. This lets you preview the UI
on Vercel immediately, then wire up Supabase later by adding the env vars
above and redeploying (no code changes needed).

## 4. Create your first Admin user

Once Supabase is connected:

1. Supabase Dashboard → **Authentication → Users → Add user** (set email + password)
2. SQL Editor:
   ```sql
   update public.profiles set role = 'admin' where email = 'you@yourgym.com';
   ```
3. Log in at `https://your-app.vercel.app/auth/login`

## Local development

```bash
npm install
cp .env.local.example .env.local   # fill in Supabase keys
npm run dev
```
