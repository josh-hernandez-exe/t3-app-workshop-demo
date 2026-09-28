# Step 4: Prepare Storage and Start

Branch: `workshop/step-04-database-setup`.

The app source is unchanged from step 3. The generated Prisma schema already describes
the tables. This step creates them on your computer and starts the page; no seed script,
demo account, database file, or generated Prisma client belongs in the checkpoint.

In a fresh clone, first follow the [recovery instructions](README.md#use-a-checkpoint-safely)
to install dependencies and configure your private `.env`. Then, inside `my-app/`:

```sh
npm run db:push
npm run dev -- --hostname 0.0.0.0
```

Open your actual port-3000 browser address. Expect **Create T3 App**, **Hello from tRPC**,
and **Sign in**. The Title input appears only after sign-in. Leave the server terminal
running; use a second terminal for other commands, or stop it with Ctrl+C.

`db:push` changes the configured database. Keep the local SQLite URL and stop on any
data-loss prompt. No records have been saved yet. Continue with
[step 5: save, reload, restart](WORKSHOP.md#5-save-reload-restart).

This checkpoint matches **Step 4: Prepare Storage and Start** in the slides.