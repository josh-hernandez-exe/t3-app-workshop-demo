# Step 3: Connect Sign-In

Branch: `workshop/step-03-connect-auth`.

The non-secret `.env.example` now includes the local `AUTH_URL`. The generated Discord
provider and Prisma adapter already supply the sign-in code, so no auth code changes
are needed for the shared workshop path.

1. In a fresh clone, run `npm ci` inside `my-app/`.
2. Follow the [private environment instructions](README.md#use-a-checkpoint-safely) if
	`my-app/.env` is missing. Generate your own `AUTH_SECRET`; keep an existing generated one.
3. Complete [step 3](WORKSHOP.md#3-connect-sign-in): use your own Discord application,
	enter its client ID and secret privately, and register the exact callback URL.
4. Set `AUTH_URL` to your real browser origin. The example's localhost address must change
	for Codespaces or a different local port. Save `.env` without committing it.

**Ready check:** your callback is saved and the private variables are filled in. A branch
cannot perform those account actions or prove that OAuth succeeds.

No database has been created yet. Continue with [step 4: open the page](WORKSHOP.md#4-open-the-page).

This checkpoint matches both **Step 3** slides: **Choose the Return Address** and
**Connect Discord Sign-In**. Working provider credentials are never supplied by a checkpoint.