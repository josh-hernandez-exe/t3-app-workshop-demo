# Workshop: Six Steps to a Working App

The `main` branch prepares your development environment. **There is no application yet.**
During the one-hour guided workshop, you will run Create T3 App, connect sign-in and storage,
and save a record through a web UI and backend. You do not need previous React, TypeScript,
database, or cloud experience to follow the shared path.

Follow the steps in order in your own copy of `main`. The [branch map](README.md) matches
each step to a reference branch. Branches describe source files, not installed packages,
configured accounts, or saved data. They are not a substitute for the ready checks below.

## Your Finish Line

1. Open your generated app in a browser.
2. Sign in and save a short project idea using its Post example.
3. Restart the app and find the record again.
4. Recognize the UI, backend, data model, and sign-in files. Understanding every line can come later.

That is enough for the core workshop. A label change is an optional first edit;
[feature exercises](EXERCISE.md) and [Vercel deployment](DEPLOYMENT.md) are follow-up paths.

## Start Here

Choose where you will work, not a framework. Unsure? Use Codespaces if your GitHub account
has access and allowance; otherwise use local Node/npm. Use Docker only if it already works
on your machine. npm comes with Node; you do not need to install Yarn or Bun.

| Environment | You need | Open |
| --- | --- | --- |
| GitHub Codespaces | GitHub account, browser, internet, available allowance | Your own repository in a Codespace; no local VS Code, Node, or Docker |
| Local Node/npm | VS Code, Git, Node 22 (includes npm) | Your cloned toolbox folder in VS Code |
| Local devcontainer | VS Code, Git, Dev Containers extension, running Docker | Your cloned toolbox folder, then **Reopen in Container**; no host Node needed |

Yarn and Bun are alternatives *inside* an environment, not a fourth kind of computer.
Use the common npm path unless you already prefer one of them.

### Two Folders, Two Jobs

```text
your-workshop-copy/       <- toolbox: install/check the generator here
	package.json
	docs/                  <- these guides
	my-app/                <- created by you during the session
		package.json         <- run database/dev commands here after init
		src/
		prisma/
```

In VS Code, open **Terminal > New Terminal**. Commands go in that terminal, not a JavaScript
file or the browser. `cd my-app` means "move this terminal into the generated app folder."
`cd ..` moves back to the toolbox. The terminal prompt should end in `my-app` for app commands.
If something fails, stop at the first error and ask; do not paste the rest of the commands.

## Before Arrival

Follow [SETUP.md](SETUP.md) for your private template copy, cloning, prerequisites, and the
Codespaces/local routes. It also has Yarn/Bun alternatives. Bring a charger and make sure
you can sign into Discord, including any two-factor verification. You may create the Discord
OAuth app beforehand; do not share its secret or initialize the T3 app early.

**Stop point:** Node 22 and the generator are ready. No `my-app/` exists. The toolbox contains
only Create T3 App 7.40.0, setup configuration, and guides, not an application or database.

## 1. Generate the App

In the **toolbox folder**, run:

```sh
npm run init -- my-app --noGit --noInstall
```

Use the arrow keys to select and Enter to confirm. Keep these choices, in wizard order:

| Prompt | Answer | Its job |
| --- | --- | --- |
| Language | TypeScript | JavaScript with checks that catch many mistakes |
| Tailwind CSS | Yes | Styling the page |
| tRPC | Yes | Calls from the page to server functions |
| Authentication | NextAuth.js | Sign-in and remembering the user |
| Database ORM | Prisma | Describe and query stored data |
| Next.js App Router | Yes | Organize pages and request handlers |
| Database provider | SQLite (LibSQL) | With Prisma, a local data file; no extra account |
| Linter/formatter | ESLint/Prettier | Code checks and formatting |
| Import alias | Keep `~/` and press Enter | A shortcut to the generated `src/` folder |

Next.js and React come with the app. The auth choice is a library, not your Discord account.
You do not need to defend each tool choice today; we use one shared path to reduce setup work.

`--noGit` avoids a nested repository. `--noInstall` separates generation from installation
so we can apply the reviewed versions below first. It does **not** skip the questions.
Do not use `--default`, `--CI`, or an arbitrary newer generator during the workshop.

**Ready check:** the wizard completes and `my-app/` contains `src/`, `prisma/`, and its own
`package.json`. Ignore the generator's generic suggestion to run `git init`: your toolbox
copy is already a repository. Do not regenerate over a nonempty folder if something fails.

Reference: `workshop/step-01-generate-app`. It contains source only; do not install its
original package versions before applying step 2.

## 2. Install the Reviewed Versions

Move into the generated app and check the package name:

```sh
cd my-app
npm pkg get name
```

Expect `"my-app"`. If you see `"cs-workshop-bootstrap"`, **stop**: you are still in the
toolbox. The following commands must not edit the toolbox's package.

The generator's old auth pin and two nested packages have published advisories. These
settings were checked on September 24, 2026; they do not change your application logic.
Apply them **before the first app install**, one command at a time:

```sh
npm pkg set "dependencies.next-auth=5.0.0-beta.32"
npm pkg set "overrides.@prisma/config.deepmerge-ts=8.0.2" "overrides.next.postcss=8.5.28"
npm install
npm audit
```

**Ready check:** install completes, Prisma reports a generated client, and the audit reports
no known vulnerabilities for this tested dependency set. A new advisory is a reason to ask
Josh, not to run `npm audit fix --force`. Do not follow Prisma's major-upgrade advertisement
during the session. Keep the generated app's lockfile.

Prisma also creates files under `my-app/generated/prisma/`. They are ignored by Git and
recreated during installation; do not edit or commit them.

Reference: `workshop/step-02-install-dependencies`. A fresh copy of this checkpoint uses
`npm ci` inside `my-app/` because its reviewed lockfile already exists.

Already installed? Stop and ask for help before applying these commands. The rehearsal
found that patching an existing install can retain stale nested packages. A new unused app
folder is a simple recovery if you have no work to preserve; do not delete existing work blindly.

Yarn/Bun users apply the corresponding pre-install recipe in [SETUP.md](SETUP.md#optional-existing-yarn-or-bun-setup)
and then rejoin below. Do not mix installers in the same app.

## 3. Connect Sign-In

An environment file holds settings for this copy of your app. In VS Code's Explorer, expand
`my-app` and open **`.env`**, not `.env.example`. The example documents names; the running
app reads `.env`. Never project or commit your real values.

| Variable | What you do |
| --- | --- |
| `DATABASE_URL` | Keep the generated `file:./db.sqlite`; it points to local storage |
| `AUTH_SECRET` | Keep the random value created by the generator; this is **not** the Discord secret |
| `AUTH_DISCORD_ID` | Copy the Client ID from your Discord developer application |
| `AUTH_DISCORD_SECRET` | Copy that application's Client Secret, not a bot token or password |
| `AUTH_URL` | **Add this line** if absent; use the base address you will open in the browser |

Locally, the added line is `AUTH_URL="http://localhost:3000"`. In Codespaces, use the actual
HTTPS address from the Ports panel. Add port 3000 with **Forward a Port** if needed to see
that address before the server starts. For a local devcontainer, check whether the forwarded
host port differs from 3000. Do not replace a generated secret with the word `placeholder`.

### Register the Return Address

1. Visit <https://discord.com/developers/applications> and choose **New Application**.
2. Open **OAuth2**. Copy the **Client ID** to `AUTH_DISCORD_ID` in the generated app's `.env`.
3. Obtain a **Client Secret** and set `AUTH_DISCORD_SECRET`. This is not a bot token.
   Resetting a secret invalidates the old one; do not reset a working application's secret casually.
4. Add the exact redirect URI, then save:

```text
http://localhost:3000/api/auth/callback/discord
```

For Codespaces, copy the HTTPS origin from port 3000 and append
`/api/auth/callback/discord`. Set `AUTH_URL` to that same origin, without the callback
path. The usual form is `https://<codespace-name>-3000.app.github.dev`; copy the actual URL.
Do not register wildcard redirects.

5. Save the redirect in Discord and save `.env` in VS Code.

OAuth means your app lets the provider handle the user's sign-in. The **callback** is the
return address after that sign-in. `AUTH_URL` is the base address; the registered callback
is that same address **plus** `/api/auth/callback/discord`. They are not interchangeable.

Never put the client secret in a `NEXT_PUBLIC_*` variable, screenshot, chat, or Git.
Each student or pair should use their own application. No Discord bot installation is required.

Prefer Google? Use the [optional Google route](SETUP.md#optional-google-instead-of-discord), with help if
needed. You only need one provider; do not follow both setup flows.

**Ready check:** the redirect is saved and your private `.env` is complete.
Reference: `workshop/step-03-connect-auth`. It adds only the non-secret `AUTH_URL` example;
you still need your own `.env`, secret, provider application, and redirect.

## 4. Open the Page

Inside `my-app/`, after configuring sign-in:

```sh
npm run db:push
npm run dev -- --hostname 0.0.0.0
```

The first command creates the local database tables. The second starts the application.
Open <http://localhost:3000> locally, or open port 3000's forwarded address in Codespaces.
On a native local installation, you may use `--hostname 127.0.0.1` instead to avoid listening
on your local network. `0.0.0.0` is a listening setting, not the URL to register with Discord.

**Ready check:** the browser shows **Create T3 App**, **Hello from tRPC**, and **Sign in**.
The hello message is already a response from backend code. Ignore the documentation cards
for now. The Post form is hidden until you sign in.

The server keeps this terminal busy; that is normal. Keep it running while using the page.
Use a second terminal for other commands, or stop it with Ctrl+C before changing `.env`
and restart with the same dev command. If port 3000 is occupied, do not close someone else's
process: choose a port with help and update the browser URL, `AUTH_URL`, and callback together.

`db:push` changes the configured database and does not create migration history. Stop and
ask before accepting any data-loss prompt. Do not point these commands at a live database.

Reference: `workshop/step-04-database-setup`. There is no seed script or committed database.
The generated schema already contains the required tables; this step runs it locally.

## 5. Save, Reload, Restart

1. Sign in through your provider and return to the app.
2. Below the sign-in area, find the **Title** input. Type `Track books I lend` and press **Submit**.
3. Look for **Your most recent post: Track books I lend**. Reload; it should still be there.
4. Stop the app with Ctrl+C. Run the same dev command again, then reload the page.
5. Confirm the record remains. You have reached the core finish line.

The example calls each record a **Post**; it is not a Discord post or a public social feed.
The UI and backend do different jobs but live in the same Next.js project. SQLite keeps
records in a file beside the app, not in the browser. The file survives a server restart;
deleting the Codespace or database file deletes that storage unless you backed it up.

Josh will show ownership with two accounts on the **same running app**. Comparing separate
students' app URLs/databases does not test ownership. Try the two-account check if you already
have the accounts; do not spend the remaining hour registering more accounts.

Reference: `workshop/step-05-save-record`. The checkpoint keeps the app unchanged and
records these checks. It cannot include your login session or prove that you saved a record.

## 6. Find Four Places

| Generated path inside `my-app/` | Responsibility |
| --- | --- |
| `src/app/_components/post.tsx` | UI: the form and latest-post display |
| `src/server/api/routers/post.ts` | Backend: handles saving and reading |
| `prisma/schema.prisma` | Data model: describes what is stored |
| `src/server/auth/config.ts` | Sign-in configuration |

Recognizing their jobs is enough today. The backend gets the owner from the signed-in
session and filters reads to that user; a hidden UI button is not a permission check.
The generated UI shows the latest Post, not a full notebook.

For a quick first edit, change the visible `Your most recent post` label in the component
and reload. For more depth, choose **one** [optional exercise](EXERCISE.md). Database auth
tables can contain tokens: do not project them or commit the database.

Reference: `workshop/step-06-final`. This completes the core workshop without adding an
optional feature. The label edit and exercises remain choices for you to make.

## Checks and Commands

Before init, at the toolbox root, only `npm run check` applies. After init, these commands
belong in `my-app/`:

```sh
npm run check
npm run build
```

With Yarn/Bun, use `yarn run check` / `bun run check` and the corresponding `run build`.
Run these **after setting the environment variables**: even lint loads the generated app's
configuration. They run lint/TypeScript and a build, not a behavior test suite. Add tests as
you develop. A warning about multiple lockfiles is expected with the toolbox/app layout;
do not delete either lockfile to silence it. Deprecated-tool notices are not instructions
to upgrade framework majors during the session.

## Troubleshooting

| Symptom | Check |
| --- | --- |
| Redirect mismatch | Exact scheme, hostname, port, callback path, saved provider settings |
| Google access denied | Consent audience, test-user list, organization restrictions |
| Sign-in loops | Consistent origin, cookies, GitHub port access, correct secret |
| App command missing at root | `cd my-app`; toolbox and app have different scripts |
| Environment validation fails | Fill generated variables; Google also needs `src/env.js` changes |
| Missing database/table | Run `npm run db:push` inside the generated app; check its URL |
| Prisma types missing | Run `npx prisma generate`, then restart TS/dev server |
| Data appears missing | Check database URL and signed-in account; latest-post UI shows one row |
| Init target exists | Choose a new folder or inspect the old one; never blindly overwrite it |
| Yarn says app is not part of the project | Create `my-app/yarn.lock`, then set the linker/install inside that folder |
| Bun blocks an install script | Inspect `bun pm untrusted`; review only what is required, never blanket-trust |
| Devcontainer does not start | Confirm Docker is running and the toolbox root is open; otherwise pair or use Codespaces |
| OAuth returns to a different port | Use the actual browser/forwarded address in both `AUTH_URL` and provider redirect |

If OAuth is unavailable, pair up and trace the generated request path. Do not remove auth
checks to work around a provider problem.

## Safety and Limits

Toolbox installation downloads CLI packages only. Student-run init writes a new app folder
and, when selected, installs its dependencies. Database changes happen only when students
run database commands. OAuth contacts external providers only after configuration/use.

The generated app is an example, not a production security review. Its default Post validator
checks only a minimum name length; stronger validation and visible failure feedback are optional
next exercises, not generated features. Discuss rate limits, permissions, backups, and costs
before opening a real service to strangers.

The unmodified generator reported seven advisories on September 24, 2026. Step 2 applies
reviewed fixes; the rehearsed npm app then passed its audit, lint, types, build, and local
browser checks. An audit is a point-in-time check, not a production security guarantee.
Public deployment still requires real-provider sign-in testing and your own data/cost review.

The toolbox lockfile does not freeze generated app dependencies. Rehearse and audit the
new app separately. Do not use `npm audit fix --force` during the session.
Codespace disk survives stops, not deletion. Back up data before deleting it, and stop
unused Codespaces to limit compute usage; retained storage still counts toward quotas.

## Explore When You Are Ready

- New to React: [React foundations](https://nextjs.org/learn/react-foundations).
- New to TypeScript: [the handbook](https://www.typescriptlang.org/docs/handbook/intro.html).
- How do the pieces connect? [T3 first steps](https://create.t3.gg/en/usage/first-steps).
- How do backend calls work? [tRPC documentation](https://trpc.io/docs).
- How does sign-in work? [Auth.js concepts](https://authjs.dev/concepts).
- How do stored records change over time? [Prisma migrations](https://www.prisma.io/docs/orm/prisma-migrate).

The pinned generator's auth example uses Auth.js v5 beta and a Prisma adapter; older NextAuth
v4 tutorials have different names. These links are invitations, not reading prerequisites.
