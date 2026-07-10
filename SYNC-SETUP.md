# Turning on account sync (computer ⇄ phone)

The app now supports signing in to an **account** so the same data appears on
your computer and your phone, and changes on one device show up on the other.

Sync is **off until you connect a free backend once**. This takes about five
minutes and you only ever do it a single time. After that, you (and any device)
just sign in from the app's **Settings → ☁️ Account & sync**.

## What you need: a free Supabase project

[Supabase](https://supabase.com) gives you the accounts + private database that
sync uses. The free tier is plenty for this app.

### 1. Create the project
1. Sign up at <https://supabase.com> and click **New project**.
2. Give it any name (e.g. `my-classes`), pick a region near you, and set a
   database password (you won't need it for the app).
3. Wait ~2 minutes for it to finish setting up.

### 2. Create the data table
1. In the project, open **SQL Editor** (left sidebar) → **New query**.
2. Copy the entire contents of [`supabase-setup.sql`](./supabase-setup.sql) into
   the editor and click **Run**. You should see "Success. No rows returned".

### 3. (Recommended) simplest sign-in
By default Supabase emails a confirmation link when you create an account.
For a personal app you can skip that:
1. Go to **Authentication → Sign In / Providers → Email**.
2. Turn **Confirm email** *off*, then Save. (Leave it on if you'd rather verify
   your email — you'll just click a link once when you create the account.)

### 4. Copy your two keys
1. Go to **Project Settings → API** (or **Data API**).
2. Copy the **Project URL** (looks like `https://abcd1234.supabase.co`).
3. Copy the **anon** / **publishable** key (a long `eyJ...` string).
   - These are *public* keys — safe to put in the app. Your data is protected by
     the row-level security rules the SQL set up. Do **not** use the
     `service_role`/secret key.

### 5. Paste the keys into the app
Open `index.html` and find these two lines near the top (in the first
`<script>` block):

```js
const SUPABASE_URL="";      // e.g. "https://abcdefgh.supabase.co"
const SUPABASE_ANON_KEY=""; // e.g. "eyJhbGciOiJIUzI1NiIs...."
```

Paste your Project URL and anon key between the quotes, save, and redeploy /
reload the app.

## Using it
1. Open the app → menu (**⋯**) → **Settings & Backup** → **☁️ Account & sync**.
2. Enter an email + password and tap **Create account** (first time), then
   **Sign in**.
3. On your phone, open the app and sign in with the **same** email + password.
4. That's it — changes now sync both ways automatically. The card shows
   **✓ Synced** when everything is up to date, and there's a **Sync now** button
   if you ever want to force a refresh.

## Notes
- **Signed out still works.** If you don't sign in, the app behaves exactly as
  before (data saved on the device, plus the optional local-file auto-save).
- **Conflicts.** If you edit the same thing offline on both devices, the most
  recently saved version wins. For one person using two devices this is rarely
  an issue.
- **Offline.** Changes are always saved on the device first, then pushed to the
  cloud the next time you're online.
