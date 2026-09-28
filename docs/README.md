# Workshop Guides

Start on `main`. It contains the generator and these guides, not a generated app.
You will create the app yourself during the workshop.

1. **Before the session:** follow [Setup](SETUP.md), then stop when the generator prints `7.40.0`.
2. **During the session:** follow the six steps in the [Workshop](WORKSHOP.md#1-generate-the-app).
3. **After the core exercise:** choose one [Exercise](EXERCISE.md) or read [Deployment](DEPLOYMENT.md).

## Match the Slides to a Checkpoint

Each checkpoint is the state **after** the matching slide step. These branches are
references and recovery points, not commands to run instead of doing the workshop.

| Slide step | Branch | What this checkpoint contains |
| --- | --- | --- |
| Before we start | `main` | Toolbox only; no app has been generated |
| 1. Create the app | `workshop/step-01-generate-app` | Generated source, before installing app packages |
| 2. Install dependencies | `workshop/step-02-install-dependencies` | Reviewed package settings and the app lockfile |
| 3. Connect sign-in | `workshop/step-03-connect-auth` | Non-secret environment example and sign-in instructions |
| 4. Prepare storage and start | `workshop/step-04-database-setup` | Instructions to create local tables and open the app |
| 5. Save, reload, restart | `workshop/step-05-save-record` | The persistence and ownership checks |
| 6. Find the four places | `workshop/step-06-final` | The completed core guide and pointers for a first edit |

Installing packages, entering credentials, creating tables, and saving a record happen
on your computer. A branch does not include your dependencies, `.env`, database, or login.
Steps 4-6 add checkpoint instructions, not extra app features or pre-filled records.

Read [this branch's checkpoint](CHECKPOINT.md) to see what is complete and what to do next.
The final branch is numbered `06` so the branches sort in workshop order.

## Use a Checkpoint Safely

You do not need to switch branches while following the workshop. A checkpoint is useful
for comparing files or recovering with Josh's help. Stop your app server first.

From your **toolbox folder**, this example creates a separate step-4 copy beside your work:

```sh
git clone --branch workshop/step-04-database-setup --single-branch https://github.com/josh-hernandez-exe/t3-app-workshop-demo.git ../cs-workshop-step-04
cd ../cs-workshop-step-04
```

Use the branch you need from the table and an unused destination name. Open the new folder
in VS Code, then follow its [Checkpoint](CHECKPOINT.md). This reference clone is not your
own GitHub repository; do not try to push changes to Josh's repository.

For every checkpoint containing an app:

1. Read the checkpoint before installing. Step 1 still needs the reviewed package settings;
	steps 2-6 already contain them and use `npm ci` inside `my-app/`.
2. If `my-app/.env` is missing, create it by copying the contents of `my-app/.env.example`.
	Never overwrite an existing `.env`. In `my-app/`, generate your own application secret:

	```sh
	node -e "console.log(require('node:crypto').randomBytes(32).toString('base64'))"
	```

3. Paste that value into `AUTH_SECRET` in your private `.env`. Do not project or share it.
	Follow [step 3](WORKSHOP.md#3-connect-sign-in) for your own Discord credentials and callback.
	The checkpoint does not supply working provider credentials or register a redirect.
4. Before running the page, follow [step 4](WORKSHOP.md#4-open-the-page) to prepare your local
	database. Before claiming completion, do the [save/restart check](WORKSHOP.md#5-save-reload-restart).

Cloning writes a new folder and downloads public source. Installation downloads packages;
`db:push` creates local tables. No checkpoint comes signed in or contains a saved demo record.
Do not delete a student folder, clear untracked files, or overwrite settings to catch up.