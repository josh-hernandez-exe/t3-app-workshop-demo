# Step 5: Save, Reload, Restart

Branch: `workshop/step-05-save-record`.

The app source is unchanged. This step proves that the existing form saves a record for
the signed-in user and that SQLite keeps it when the app process stops. Checking out a
branch does not perform that test or supply a login session.

In a fresh clone, use the [recovery instructions](README.md#use-a-checkpoint-safely) to
install dependencies, configure your private `.env`, create local tables, and start the app.
Then complete [workshop step 5](WORKSHOP.md#5-save-reload-restart):

1. Sign in through Discord and return to your app.
2. In the Title input, enter `Track books I lend` and press **Submit**.
3. Look for **Your most recent post: Track books I lend**, then reload.
4. Stop the dev server with Ctrl+C, run the same dev command again, and reload.
5. Confirm the same record remains. No seed command is needed.

The starter displays only your latest Post, not every saved row. Josh's two-account demo
uses the **same app URL and database**: a second user must not see the first user's record.
Separate students' databases do not prove ownership checks work.

Your environment, database, and account/session records stay local and out of Git.
Continue with [step 6: find four places](WORKSHOP.md#6-find-four-places).

This checkpoint matches **Step 5: Sign In and Save Something** in the slides.