# Before the Workshop: Choose One Environment

This is preparation, not the workshop itself. Stop when Create T3 App reports `7.40.0`.
Do not generate `my-app` before the session. The [main walkthrough](README.md) starts there.

## Pick a Route

| Route | Required on your computer | Account requirement |
| --- | --- | --- |
| Codespaces | A web browser and internet | GitHub account with Codespaces access and allowance |
| Local Node/npm | VS Code, Git, Node 22 (includes npm) | No GitHub login to clone a public repo; one is needed to publish your own copy |
| Local devcontainer | VS Code, Git, Dev Containers extension, running Docker | Same GitHub distinction as local Node; no host Node installation needed |

Unsure? Choose Codespaces if you have access, otherwise local Node/npm. Do not install
Docker or change package managers during the event just to match someone else's setup.
All routes need one sign-in-provider account, normally Discord. No school account, campus
software license, or payment card is required for the core exercise. Pair work is welcome.

Node runs JavaScript outside the browser, including this app's backend. npm downloads the
reusable packages the project needs. VS Code is the editor and terminal window; it does
not start the app by itself. A container supplies these tools in a consistent environment.

## Get Your Own Copy

Open the organizer's public template link. In GitHub, choose **Use this template > Create
a new repository**, give it a name such as `cs-workshop`, and choose **Private** for practice.
Use that new repository for the instructions below.

This creates your own copy, not a fork. A GitHub fork of a public repository cannot simply
be made private. A private template-created repository is the appropriate rehearsal route.

For local work without a GitHub account, you can clone the public template URL directly.
You do not need to publish anything during the lab. Do not try to push to the instructor's
repository; make your own repository later if you want to share your work.

## A. GitHub Codespaces

1. In **your copy**, open **Code > Codespaces > Create codespace**.
2. Let the environment finish creating. The supplied configuration runs
   `npm ci && npm run check`, which installs and checks the generator only.
3. Open **Terminal > New Terminal**. Run `node --version`, then `npm run check`.
4. Expect `v22.x` and `7.40.0`. Stop here. There should not be a `my-app/` folder yet.

No local VS Code, Docker, or Node is needed. Codespaces is the remote computer; closing a
browser tab does not necessarily stop it. Stop unused Codespaces from GitHub's Codespaces
page. Check your allowance before the event; compute and retained storage can incur charges.

If allowance or organization policy blocks creation, use a local route or pair up. There
is no expectation to add a card to recover from a setup problem.

Later, keep port 3000 **private**. Its HTTPS browser address is different from localhost.
The browser returning from Discord must also be signed into GitHub to pass the private-port
access check. GitHub port access and your application's sign-in are two separate things.

## B. Local Node and npm

1. Install [VS Code](https://code.visualstudio.com/), [Git](https://git-scm.com/downloads),
   and [Node 22](https://nodejs.org/en/download/archive/v22.23.2). npm is included with Node.
   Use Node 22 for the shared path; do not choose an unrelated newer major during the session.
2. Copy the repository's HTTPS URL from **Code** on GitHub. In VS Code's Command Palette,
   run **Git: Clone**, paste the URL, choose a location, and open the cloned folder.
3. Confirm the Explorer shows the toolbox's `package.json`, `README.md`, and `.devcontainer`.
   You should have this toolbox open, not the whole teaching repository or your home folder.
4. Open **Terminal > New Terminal**. If you installed Node after opening VS Code, restart
   VS Code first so the terminal can find it.
5. Run these one at a time:

```sh
node --version
npm ci
npm run check
```

Expect Node `v22.x` and generator `7.40.0`. `npm ci` uses the supplied lockfile and downloads
the toolbox's dependencies. It does not create your app. Stop here until the workshop.

If you already use nvm, `nvm install` and `nvm use` select the supplied `.nvmrc` version.
Do not learn a new version manager just for the event. On Windows, if PowerShell refuses
`npm.ps1`, use `npm.cmd` in place of `npm`, or select **Command Prompt** as the VS Code
terminal profile. Do not weaken a school/work execution policy to run the workshop.

No Docker is needed. VS Code is the shared editor so everyone can follow the file navigation;
an experienced developer can use another editor, but the live instructions use VS Code.

## C. Local Devcontainer

1. Install VS Code, Git, the
   [Dev Containers extension](https://marketplace.visualstudio.com/items?itemName=ms-vscode-remote.remote-containers),
   and [Docker](https://www.docker.com/get-started/). Start Docker. Follow its platform
   requirements, including WSL 2 where applicable on Windows, before arriving.
2. Clone and open the toolbox as in route B. Run **Dev Containers: Reopen in Container**
   from the Command Palette. Use the included config, not **Add Configuration Files**.
3. Allow the first image download and post-create command to finish. VS Code should show
   that it is connected to the container.
4. Open a **new container terminal** and run `node --version` and `npm run check`.
   Expect `v22.x` and `7.40.0`. Stop here; no app has been generated.

The image supplies Node/npm, so you do not need a second Node installation on your host.
Docker must be running; installing only the VS Code extension does not install Docker.

The container forwards port 3000. Check the **Ports** panel for its actual local address;
another host process may force a different port. Use that same address in the browser and
auth settings. Forwarding a port is not deploying the app to the public internet.

The toolbox must be the opened root folder. While this folder is inside the teaching repo,
open it in a separate VS Code window for this route. GitHub Codespaces uses the published
toolbox's root configuration, not a nested configuration in the teaching repository.

## Optional: Existing Yarn or Bun Setup

These are package managers, not additional environment routes. Keep Node 22 installed and
use one manager per generated app. The parent toolbox keeps its own npm lockfile.
Do not switch a partially installed app between managers during the workshop.

### Yarn 4

Use modern Yarn 4, not Yarn Classic. Before the session, check the pinned generator without
creating an app:

```sh
node --version
yarn --version
yarn dlx create-t3-app@7.40.0 --version
```

During the session, from the toolbox folder:

```sh
yarn dlx create-t3-app@7.40.0 my-app --noGit --noInstall
cd my-app
```

Answer the same nine questions in the main walkthrough, including the `~/` import alias.
Create an empty **`yarn.lock` inside `my-app/`** using VS Code's New File command. This
marks the generated app as a separate project, not a missing workspace of the toolbox.

Apply the reviewed package settings before the first install, then use the regular
`node_modules` linker for this workshop:

```sh
npm pkg set "dependencies.next-auth=5.0.0-beta.32"
npm pkg set "resolutions.deepmerge-ts=8.0.2" "resolutions.postcss=8.5.28"
yarn config set nodeLinker node-modules
yarn install
```

Here `npm pkg set` only edits JSON metadata; it does not install packages or create an npm
lockfile. Use Yarn for this app's installs afterward. Continue at **Connect Sign-In** in the
main walkthrough, using `yarn run db:push` and `yarn run dev --hostname 0.0.0.0` for the app.
Use `yarn run check`, `yarn run build`, and `yarn npm audit --recursive` when checking it.
Yarn's audit also lists package deprecations: the rehearsal reported ESLint 9's deprecation
even after the known security advisories were resolved. Do not interpret every notice as
a failed install, or dismiss a different/new advisory because this one was expected.

### Bun

Use Bun as the package manager, with Node 22 still available for the generated tools.
Before the session:

```sh
node --version
bun --version
bunx create-t3-app@7.40.0 --version
```

During the session:

```sh
bunx create-t3-app@7.40.0 my-app --noGit --noInstall
cd my-app
npm pkg set "dependencies.next-auth=5.0.0-beta.32" "devDependencies.postcss=8.5.28"
npm pkg set "overrides.deepmerge-ts=8.0.2" "overrides.postcss=8.5.28"
bun install
```

`npm pkg set` edits JSON only. Continue with the common sign-in instructions, then
`bun run db:push` and `bun run dev --hostname 0.0.0.0`. Use `bun run check`, `bun run build`,
and `bun audit` for checks. Do not add `--bun` to force a different runtime.

If Bun reports blocked install scripts, inspect `bun pm untrusted` with a helper. Do not
blanket-trust every package. Keep the generated lockfile and any deliberately reviewed
trust settings. `bun install` can run project scripts and download Prisma engine files.

## If Preparation Stops

| What you see | First action |
| --- | --- |
| `node` / `npm` not found | Install Node 22 or open the correct container terminal; restart VS Code afterward |
| `ENOENT` / no `package.json` | Open the toolbox folder; check the terminal's current folder |
| `npm ci` says lockfile missing | Check you copied the whole template, including its lockfile; do not invent an app yet |
| Docker daemon unavailable | Start Docker; if unavailable, use local Node or Codespaces |
| Yarn says app is outside the project | Create the generated app's own `yarn.lock` before its install |
| Target folder already exists | Inspect it or choose an unused name; never select an overwrite/delete option casually |
| Slow downloads or quota limits | Stop retrying repeatedly; ask for help or pair with a working setup |

All setup routes write local files and download packages. No route provisions a database
service, initializes the student app automatically, or creates cloud accounts for you.
Return to the [main walkthrough](README.md) when the generator is ready.

## Optional: Google Instead of Discord

Do this after generating your app, only if Google is your chosen alternative. Do not set up
both providers just to follow the lab; Discord is the group walkthrough.

1. In <https://console.cloud.google.com/>, create or choose a project you control.
2. Configure OAuth consent: app name, support contact, and audience. For an external app in
   testing, add your Google account as a test user.
3. Create a **Web application** OAuth client. Register your actual app base address plus
   `/api/auth/callback/google`. Locally this is `http://localhost:3000/api/auth/callback/google`.
4. Set `AUTH_GOOGLE_ID` and `AUTH_GOOGLE_SECRET` in the generated app's `.env`.
5. Replace the Discord import/provider in `src/server/auth/config.ts` with Google from
   `next-auth/providers/google`.
6. Replace the Discord variable names with Google's in **both** the server schema and
   `runtimeEnv` in `src/env.js`. Update the non-secret `.env.example` names too.
7. If your generated page contains an explicit `signIn("discord")` call, replace it with
   `signIn("google")`. The tested scaffold uses a generic sign-in link and needs no such edit.
8. Check `AUTH_URL`, restart the app, and sign in with the approved test account.

If asked for a JavaScript origin, use the base address without the callback path.
Organization policies may block consent. Pair with a working setup instead of disabling
security checks. Rejoin at [Open the Page](README.md#4-open-the-page).