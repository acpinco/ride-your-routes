# Public feedback repo: setup and sync

## Why this design
GitHub repos are all-or-nothing for code: if the repo is public, the code is public. So the public repo is a separate, nearly empty repo (README, issue forms, releases). The private repo stays private, always. Don't change its visibility and don't fork it.

## Step 0: rename the private repo (optional, do it first)
1. github.com/acpinco/ride-your-rides > Settings > General > Repository name: `ride-your-routes-private` > Rename. It stays private.
2. GitHub redirects the old URL, so nothing breaks right away. Still, update your local checkout so it points at the new name:
   `git remote set-url origin https://github.com/acpinco/ride-your-routes-private.git`
   then `git remote -v` to confirm. (Why: redirects work but are not guaranteed forever.)
3. Do it between branches, not mid-task. The open branch `claude/ride-log-data` is unaffected, but the Claude project's repo link may need re-attaching afterward.
4. Don't create a new repo named `ride-your-rides` later; that would take over the old redirect.

## One-time setup (you do this on github.com)
1. New repository: Owner `acpinco`, name `ride-your-routes`, **Public**, tick "Add a README". No license needed yet (no code in it). Note: the name is visible to everyone, so pick one you like.
2. Add the files from this folder, either by drag and drop on the repo page ("Add file" > "Upload files"), or by cloning and committing:
   - `README.md` (replace the generated one)
   - `.github/ISSUE_TEMPLATE/config.yml`, `bug_report.yml`, `feature_request.yml`
   Commit straight to main; this repo has no code, so branches and PRs add nothing.
3. Settings > General > Features: keep **Issues** on; turn **off** Wiki, Projects and Discussions (less to moderate). Pull requests can stay on; with no code in the repo there is nothing to merge, and nobody can push without your invite.
4. Issues > Labels: confirm `bug` and `enhancement` exist (they do by default). Optionally add `fixed-in-next-release` and `wontfix`.
5. Check the result while logged out (private window): you should see the README and the two issue forms, and no blank-issue option.
6. Before pointing anyone at it, search the repo for your email, Strava export, or screenshots with your photo. The files in this folder have none.

## Keeping it in sync
"Sync" here is not mirroring files. It is a link between issues and your private work. Simplest version, no automation:

- **Triage**: public issues are the single list of bugs and feature requests. Don't copy them into the private repo.
- **Reference**: when you fix one in the private repo, put the public issue in the PR description, for example `Fixes acpinco/ride-your-routes#12`. GitHub closes the public issue when the private PR merges into main, because you have write access to both. (Closing keywords need the full `owner/repo#number` form across repos.) The squash commit message is what counts, so keep that line in the final message.
- **Release notes**: each time you ship, create a Release in the public repo (Releases > Draft a new release), tag like `v0.3.0`, and paste the user-facing changes. This is the changelog people see.
- **APK (optional)**: you can attach the signed release APK to that public release so testers can install it. Only do this with a release-signed build, never a debug build, and never attach keystores or `local.properties`. Skip it until you want outside testers.
- **Optional later**: a GitHub Action in the private repo that creates the public release automatically needs a personal access token stored as a secret. That is more to maintain and a security risk if mistakes are made, so I'd hold off until the manual way gets tedious. If you want it, it would be one branch in the private repo, done after the current one is merged.
