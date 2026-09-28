# Software Factory

Put a `ready` label on a GitHub issue. A coding agent picks it up inside an Upstash Box, makes the change and opens a pull request. You review it and merge it yourself.

Repos, workers and agent types are listed in [factory.config.json](factory.config.json).

<!-- Stage 9 of the skill fills this in: the diagrams, how to add a repo, a
worker or an agent type, how to read the labels, and the everyday commands. -->

## Setting up

1. set the `.env` vars
2. `npm ping`
3. `npm install --verbose`
4. `node --env-file=.env scripts/check-setup.mjs`
5. `node --env-file=.env scripts/smoke-test.mjs claude --yes`
6. `node --env-file=.env scripts/install-trigger.mjs --yes`
   
   This script:
   - Checks your GitHub token permissions
   - Adds webhook to the app repo
   - Creates labels (`ready`, `factory:running`, `factory:review`, `factory:needs-attention`)
   - Installs the workflow file
   - Merge
7. Provision Workers
   After that, set up your worker pool:

   - `node --env-file=.env scripts/provision-workers.mjs --yes`
   - This reads `factory.config.json` and creates the Boxes for your workers (currently 1 Box named `factory-claude-01`).
   
8. Test with Demo Issues
   - Create sample issues to test end-to-end:
   - Go to https://github.com/mm-mazhar/<target-repo>/settings
   - Scroll to "Features"
   - Check the Issues checkbox
   - Save
   - `node --env-file=.env scripts/create-demo-issues.mjs --yes`

## Everyday commands

```bash
node --env-file=.env scripts/status.mjs
```

```bash
node --env-file=.env scripts/release-worker.mjs <worker id>
```

if worker needs to be re-built

```
node --env-file=.env scripts/build-snapshot.mjs <worker id> --yes
```

```bash
node --env-file=.env scripts/apply-box-settings.mjs
```

Delete the old worker Box

```
node --env-file=.env scripts/delete-workers.mjs --yes
```

The first shows who is free and who is busy. The second frees a worker that a cancelled run left marked busy. The third writes a changed model or effort setting into the Boxes that already exist.

## Adding Another Repo Later

1. Add to `factory.config.json`:
   
   ```
   "repos": [
     {
       "repo": "mm-mazhar/cli",
       "baseBranch": "master",
       "setup": ["make all"],
       "checks": ["make codestyle", "make test"],
       "exclude": []
     },
     {
       "repo": "mm-mazhar/cal.diy",  // Add this
       "baseBranch": "main",
       "setup": ["npm ci"],
       "checks": ["npm test"],
       "exclude": []
     }
   ]
    ```
2. Extend GitHub token to include the new repo's access
3. Run (once):
   ```
    node --env-file=.env scripts/install-trigger.mjs --yes
    node --env-file=.env scripts/set-secrets.mjs --yes
   ```