# Step 2: Install the Reviewed Dependencies

Branch: `workshop/step-02-install-dependencies`.

The generated app now has the reviewed NextAuth version, dependency overrides, and an
npm lockfile. No application behavior has changed. Installed packages and Prisma's
generated client stay on your computer, not in Git.

In a fresh clone, install the recorded versions from the toolbox folder:

```sh
cd my-app
npm ci
npm audit
```

Expect Prisma to generate its client and the audit to report no known vulnerabilities
for the reviewed lockfile. Ask Josh about any new finding; do not use `audit fix --force`.

There is no configured sign-in, database, or running server yet. Continue with
[step 3: connect sign-in](WORKSHOP.md#3-connect-sign-in). For a fresh clone, first follow
the [private environment instructions](README.md#use-a-checkpoint-safely).

This checkpoint matches **Step 2: Install in the App Folder** in the slides.