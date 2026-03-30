# banda-1663-zola

Test repo for [BANDA-1663](https://jira.cfdata.org/browse/BANDA-1663): verifying that Zola is preinstalled in the Cloudflare Pages v2/v3 build image.

## Background

Zola was preinstalled in the v1 Pages build image but was dropped in v2/v3. With v1 being deprecated on September 15, 2026, Zola users will have broken builds when auto-migrated. The fix adds Zola back as a preinstalled tool in the v2 base image (which v3 inherits).

The relevant change is in [cloudflare/ew/pages-infra!1167](https://gitlab.cfdata.org/cloudflare/ew/pages-infra/-/merge_requests/1167).

## How this repo was created

Following the official [Cloudflare Pages Zola deploy guide](https://developers.cloudflare.com/pages/framework-guides/deploy-a-zola-site/):

1. Installed Zola locally via `brew install zola`
2. Ran `zola init banda-1663-zola` with the following answers:
   - URL: `https://banda-1663-zola.pages.dev`
   - Enable Sass compilation: `Y`
   - Enable syntax highlighting: `y`
   - Build a search index: `y`
3. Added minimal `templates/base.html` and `templates/index.html` so `zola build` produces output
4. Added `.gitignore` to exclude the `public/` build output directory

## Testing plan

The goal is to confirm that a Zola site builds successfully on the **v2/v3 build image without `ZOLA_VERSION` set**.

### Steps

1. Connect this repo to a Cloudflare Pages project pointed at the **staging** build image
2. Trigger a build with the settings below — **do not set `ZOLA_VERSION`**
3. Confirm the build passes (Zola is found and runs successfully)
4. Confirm the deployed site loads at the Pages URL

### Expected result

- **Before the fix**: build fails with `zola: command not found`
- **After the fix**: build succeeds, site deploys

### Cloudflare Pages settings

| Setting                | Value        |
| ---------------------- | ------------ |
| Build command          | `zola build` |
| Build output directory | `public`     |
| `ZOLA_VERSION`         | *(not set)*  |
