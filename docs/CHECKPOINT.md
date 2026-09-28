# Step 6: Find the Four Places

Branch: `workshop/step-06-final`.

This is the final **core workshop** checkpoint. The app source is unchanged from step 3:
steps 4-6 run it, verify persistence, and identify where future changes belong. No optional
exercise, seed script, demo login, or deployment has been added.

In a fresh clone, use the [recovery instructions](README.md#use-a-checkpoint-safely) to
install dependencies, configure your private `.env`, create local tables, and start the app.
Complete the [save/restart check](WORKSHOP.md#5-save-reload-restart) yourself before claiming
that your copy works. A branch does not include your credentials or saved data.

## Identify the Four Responsibilities

| File | What to find |
| --- | --- |
| [UI](../my-app/src/app/_components/post.tsx) | The Title input, Submit button, and call to `post.create` |
| [Backend](../my-app/src/server/api/routers/post.ts) | Protected create/read procedures; ownership comes from the session |
| [Data model](../my-app/prisma/schema.prisma) | The Post fields and relation to User |
| [Sign-in](../my-app/src/server/auth/config.ts) | Discord provider and Prisma adapter |

Point to where the page asks to save and where the server handles that request.
The label still says **Your most recent post**. Changing it is an optional first edit,
not a feature already completed for you.

After configuring the environment, check your app from inside `my-app/`:

```sh
npm run check
npm run build
```

These check code and build the app; they do not replace the browser checks.
Choose one [optional exercise](EXERCISE.md) or read [Deployment](DEPLOYMENT.md) next.
Deployment is a separate follow-up that needs hosted storage and real-provider rehearsal.

This checkpoint matches **Step 6: Find the Four Places** in the slides. The `06` keeps
the final branch in the same alphabetical order as the other numbered checkpoints.