# Optional Next Steps: Make the Project Yours

First finish the [core setup](README.md#5-save-reload-restart): sign in, save, reload, and restart.
Then choose one task below based on your comfort level. You are not expected to finish all
of these during the workshop. No exercise implementation is prebuilt in the toolbox.

Work in the generated `my-app/` folder. npm commands are shown; use `yarn run` or `bun run`
for the same scripts if that is the package manager you chose.

## 1. First Edit: Change a Label

1. Open `src/app/_components/post.tsx`, the form and latest-post display.
2. Find the visible text `Your most recent post`. Change it to a label for your own idea,
   such as `My next project`. Do not rename variables or change the save function yet.
3. Reload the page, confirm the label changed, and save another record.
4. Point to the backend file `src/server/api/routers/post.ts`. You do not need to modify
   it today; recognize that a UI change and a data-handling change are different jobs.

The example shows only the most recent Post, not a complete notebook. The generator connects
the pieces; it does not decide the product you should build. This small edit is a first
step toward using the tool, not a claim that you have mastered the whole stack.

## 2. Curious About the Backend? Improve Input Checks

The generated `post.create` input checks `name` with `z.string().min(1)`. That accepts
whitespace-only names and has no upper length limit. Its example component has a pending
state but no explicit mutation error message.

1. Predict the result of sending `"   "`, an empty string, and a very long name.
2. Trim the name and enforce a 1-100 character length on the server. Retain the protected
   procedure and derive `createdBy` from the session, never from submitted input.
3. Add a matching input limit and a proper label in the component. Explain why this does
   not replace server validation.
4. Display the mutation error and keep the text after failure. Clear it only on success.
5. Verify whitespace rejection, a too-long request, normal saving, and signed-out access.
   Inspect the tRPC response; this app does not have an `/api/ideas` endpoint.

## 3. Ready for a Data Change? Record a Next Step

1. Add `nextStep String?` to the generated `Post` model. Optional data lets old rows remain valid.
2. Run `npm run db:push` on your disposable development database, then regenerate the client
   if needed with `npx prisma generate`. Restart the dev server after schema changes.
3. Add an optional string with a 300-character limit to the create input. Decide how blank
   text is stored. Include the validated field explicitly in Prisma's create data.
4. Add a labelled form field, send it in the tRPC mutation, and display it with the latest Post.
   The inferred return type should reflect that existing records can have `null` values.
5. Test empty, valid, oversized, and previously stored values. Run `npm run check`.
6. Explain why this field helps your chosen user. Then consider renaming Post for your domain.

`db:push` does not create a migration history. Discuss migrations as a next step before
sharing or deploying the database. Do not run a reset to bypass drift on data you want to keep.
There is no bundled PostgreSQL schema; deployment requires adapting the student's schema.

## Further Challenges

- Replace ideas with a reading queue or experiment notes. Rename the domain carefully.
- Add owner-only update/delete. Include both record ID and session owner in the database
  condition; test that a different user cannot alter a guessed ID.
- Add a status and filter. Decide how old records receive a default status.
- Add pagination without exposing another user's rows.
- Explain which errors tRPC's shared types catch, and add tests for the runtime rules they cannot enforce.

## Explain Your Work

1. What problem does the feature solve, and for whom?
2. What decision did you make that the starter did not make for you?
3. What invalid or unauthorized request did you test?
4. What is the smallest useful improvement you will make this week?

Any one small, working change is a useful next step. You do not need to complete a platform
or understand every framework before continuing. Revisit these questions as your project grows.

For background, try [React foundations](https://nextjs.org/learn/react-foundations),
[tRPC](https://trpc.io/docs), or [Prisma's data model guide](https://www.prisma.io/docs/orm/prisma-schema/data-model).