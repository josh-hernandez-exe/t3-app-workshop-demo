# Bonus: Deploy to Vercel

This guide applies after you initialize and develop your own T3 app. The toolbox itself
is not deployable and includes no application or database migrations. Deployment is optional;
review current pricing, quotas, regions, and billing controls before creating cloud resources.

This is a follow-up exercise, not a required task in the one-hour lab. Finish sign-in,
saving, and persistence first. In the room Josh may show a prepared deployment, not ask
everyone to migrate a database in the final few minutes.

**Before public deployment:** complete the [reviewed pre-install step](WORKSHOP.md#2-install-the-reviewed-versions)
and recheck your chosen package manager's audit. The unmodified scaffold has older packages
with known advisories. The September 24 npm rehearsal cleared those findings after the
documented settings, but that is not a production security guarantee. Rehearse real provider
sign-in and ownership; never use `audit fix --force` as a shortcut.

## 1. Prepare the Repository

1. Commit your generated application and its lockfile to your own repository. If the app is
   still nested in this toolbox, Vercel's Root Directory must be `my-app`, not `.`.
2. Confirm `.env`, database files, `node_modules`, `.next`, and `generated/prisma` are not tracked.
3. Inside the app, run `npm run check` and audit its dependencies. Complete sign-in,
   ownership, persistence, validation, and failure-feedback checks.
4. Do not commit `.env.production` or paste database URLs into shell commands or chat.

## 2. Provision PostgreSQL

Create a separate managed PostgreSQL database, through a supported Vercel Marketplace
integration or directly with a provider such as Neon. Vercel functions do not make a
local SQLite file durable.

Follow the provider's instructions for TLS and pooled/runtime versus direct/migration
URLs. Keep application and database regions close where practical. Limits and pooling
choices vary by provider; do not assume every free plan behaves alike.

Use separate disposable development and production databases. Put each connection URL in
the appropriate ignored environment file using your private editor. Use a direct connection
for migrations if the pooler does not support them. Do not use SQLite migration history
against PostgreSQL. No cloud provisioning is performed by the toolbox's install command.

## 3. Adapt Your Schema and Create Migrations

1. Back up local data. In the generated app's `prisma/schema.prisma`, change the datasource
   provider from `sqlite` to `postgresql`. Keep your Post/User/auth models and custom fields.
2. Point the development `DATABASE_URL` at your new, empty **development PostgreSQL** database.
   The core exercise used `db:push`, so it has no migration history. If you already created
   SQLite migrations, preserve them separately and plan a provider-specific migration history.
3. In the generated app, create and review an initial PostgreSQL migration:

```sh
npx prisma migrate dev --name initial
npx prisma generate
```

With Yarn, use `yarn exec prisma` in place of `npx prisma`. With Bun, use
`bunx --no-install prisma`. These must use the generated app's installed Prisma version.

4. Commit the PostgreSQL schema and reviewed migrations. Test locally against the development
   database. This process does not transfer your SQLite rows.
5. Put the production migration URL in ignored `.env.production` as `DATABASE_URL`, then
   apply the reviewed migrations from the generated app directory:

```sh
node --env-file=.env.production node_modules/prisma/build/index.js migrate deploy
```

These commands modify the configured databases. Never run `migrate dev`, `migrate reset`,
or `db push --accept-data-loss` against production. Do not attach production migrations to
every preview build. Rehearse the provider switch separately; it is not a one-click deploy.

## 4. Configure Vercel

1. Import the repository with the Next.js preset. Set Root Directory to the folder containing
   the **generated app's** package, usually `my-app`. If extracted again into its own repo,
   use `.`. Never select the toolbox's package as the application.
2. Use Node **22.x** and the commands for your generated app's package manager below.
   Commit its matching lockfile. The schema must already target PostgreSQL.
3. Add the production-scoped variables listed after the command table.

| Manager | Install | Build |
| --- | --- | --- |
| npm | `npm ci` | `npx prisma generate && npm run build` |
| Yarn 4 | `yarn install --immutable` | `yarn exec prisma generate && yarn run build` |
| Bun | `bun install --frozen-lockfile` | `bunx --no-install prisma generate && bun run build` |

Keep Yarn's `node-modules` configuration with the app. Keep any reviewed Bun install-script
trust settings with the app too. Rehearse these on the hosting provider; do not change
package managers during deployment.

| Variable | Value |
| --- | --- |
| `DATABASE_URL` | Provider's runtime PostgreSQL connection URL |
| `AUTH_URL` | Stable production HTTPS origin, without callback path |
| `AUTH_SECRET` | Fresh strong production secret, not the local one |
| `AUTH_DISCORD_ID`, `AUTH_DISCORD_SECRET` | Production Discord credentials for the default config |
| `AUTH_GOOGLE_ID`, `AUTH_GOOGLE_SECRET` | Google credentials if you changed the provider/config |

Generate the production secret in a password manager. No secrets use `NEXT_PUBLIC_`.
Restrict production secrets to production. Keep preview deployments on a separate database.
The migration connection can remain outside Vercel. These names match the generator's
Auth.js v5 example; check the generated environment schema after any customization.

4. Create a separate production OAuth application/client. Register the stable origin plus
   `/api/auth/callback/discord` or `/api/auth/callback/google`.
5. Deploy. If the production domain is assigned on first deployment, update the URL and
   callback afterwards, then redeploy. Domain changes need the same coordination.

## 5. Verify and Operate

1. Open the production URL in a fresh browser session. Sign in and save a test record.
2. Reload, sign out/in, and confirm persistence. A second account must not see the record.
3. Redeploy without resetting storage; confirm the saved record still exists.
4. Review logs without logging cookies, OAuth tokens, or database connection strings.
5. Check backups, restore procedure, connection limits, and spending alerts.

Preview OAuth needs a stable separate domain/client/database or a reviewed provider-supported
workflow. Random per-commit URLs do not automatically match callbacks. Do not use wildcard
redirects or share production secrets as a workaround.

Stop unused paid resources, but export anything you want to keep before deleting databases.
This is a draft deployment path: live consent, fresh Codespace startup, and an actual
PostgreSQL/Vercel deployment still need rehearsal with your accounts.