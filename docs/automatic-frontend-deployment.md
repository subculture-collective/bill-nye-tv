# Automatic public frontend deployment

On GitHub, the `Mirror CI` workflow runs the full repository verification
(`make verify`) for every pull request branch, then builds the frontend image
and checks its nginx configuration. Those builds receive no registry or
deployment credentials. Pushes and manual dispatches of main run the `Deploy`
workflow: it joins the tailnet, builds the image, publishes it to GitHub
Packages with the workflow token, then streams the same frontend image to the
existing Almaz service. The Gitea `verify` workflow only repeats `make verify`
on main after Kvant copies it there.

This app uses Compose service `bill-nye-reference`, container `arkhiv-platform-public-20261004-bill-nye-reference-1`, at
https://bill-nye.subcult.tv/. Deployment uses this repository's dedicated `HOMELAB_DEPLOY_KEY` and
pinned `HOMELAB_KNOWN_HOSTS`. The forced-command receiver is maintained in the
private `subculture-collective/fleet-deploy` repository. Its local profile permits
only this app and frontend service. It refuses stale commits and wrong-app
images, retains the live Compose configuration, verifies image identity,
resources, runtime overrides and public HTML, and restores the previous image
if readiness fails. CI also compares the full public HTML with the image it
built. Private host receipts retain exact prior image IDs and rollback commands.

The homelab operator owns the key; its first rotation deadline is 2027-02-03.
Register and test the receiver/key before merging the pipeline. Keep prior
images, deployment files and receipts for recovery. Raw failure diagnostics stay
in mode-0600 host files and must not be attached to PRs or CI artifacts.

The shared core pins, database, media/archive mounts, public collection review,
original source metadata and engine release procedures retain their own
controls. This pipeline updates the frontend of the already public app; it does
not acquire content or publish additional material. Browser playback/reading,
audiovisual review, independent restore and deployment remain separate evidence.

The public image sets `CORE_UPSTREAM` to `bill-core:8080` at build time,
using Docker DNS to reach the existing collection-specific API. The default
`api:8080` remains suitable for the isolated local stack. On Almaz the receiver
uses a locally reviewed frontend-only Compose baseline and its registered runtime
guard, so it can deploy without reading sibling services’ private env files.

Before building on main, CI queries the receiver for an already accepted image
for that exact commit. A retry reuses that immutable image and its public HTML
hash, skipping image rebuild/publication. The receiver deploys only the
revision that is Gitea's current main, so the job waits up to ten minutes for
the copy from GitHub to arrive. PRs still build and run the nginx
configuration check, and the `Mirror CI gate` check is required to merge.
