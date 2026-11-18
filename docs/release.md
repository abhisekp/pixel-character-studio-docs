# Release automation

The source repository is private; this documentation repository is public. Documentation deploys to GitHub Pages on each main-branch push using GitHub Actions.

Tag the source repository `v0.4.0` (matching package and Tauri versions) to run tests, build a Windows installer, sign updater assets, upload workflow artifacts and create a release. The workflow uses Node 24 and Rust stable. Release jobs require `TAURI_SIGNING_PRIVATE_KEY` in source Actions secrets. The public updater key is committed in Tauri configuration; never rotate it casually because installed clients trust it.

For automatic mirroring to this public release repository, configure a narrowly scoped `PUBLIC_RELEASE_TOKEN` with Contents write permission on `pixel-character-studio-docs`. Without it, the workflow fails before publishing and reports the required setup. A broad personal credential is not copied into CI automatically.

Optional Windows publisher signing uses `WINDOWS_CERTIFICATE_BASE64`, `WINDOWS_CERTIFICATE_PASSWORD` and `WINDOWS_CERTIFICATE_THUMBPRINT`. The PFX is installed in the ephemeral Windows runner certificate store, never committed or uploaded. No paid signing service is provisioned automatically.

This documentation describes the intended release contract. Check workflow runs and release assets for actual execution results. Signed OTA checks are implemented; installation between two distinct production versions still requires an end-to-end test.
