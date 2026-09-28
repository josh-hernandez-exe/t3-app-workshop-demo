# Step 1: Generate the App

Branch: `workshop/step-01-generate-app`.

The interactive wizard has created `my-app/` with TypeScript, Tailwind, tRPC, NextAuth,
Prisma, App Router, SQLite, ESLint/Prettier, and the `~/` alias. The app's dependencies
have not been installed. The application README points back to the workshop guides.

1. Open `my-app/` and find `src/`, `prisma/`, and its `package.json`.
2. Do not run the generator again in this copy. Do not install the original package versions.
3. Continue with [step 2: install the reviewed versions](WORKSHOP.md#2-install-the-reviewed-versions).

There is no database, sign-in session, or running server yet. A fresh clone has no private
`.env`; use the [checkpoint recovery instructions](README.md#use-a-checkpoint-safely)
when you reach sign-in setup. Never commit your environment file.

This checkpoint matches **Step 1: Create the App Yourself** and **The Wizard: In This Order**
in the slides. Read the [branch map](README.md#match-the-slides-to-a-checkpoint) for all steps.