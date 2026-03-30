# banda-1663-zola

Test repo for [BANDA-1663](https://jira.cfdata.org/browse/BANDA-1663): verifying that Zola is preinstalled in the Cloudflare Pages v2/v3 build image.

## Background

Zola was preinstalled in the v1 Pages build image but was dropped in v2/v3. With v1 being deprecated on September 15, 2026, Zola users will have broken builds when auto-migrated. The fix adds Zola back as a preinstalled tool in the v2 base image (which v3 inherits).

The relevant change is in [cloudflare/ew/pages-infra!1167](https://gitlab.cfdata.org/cloudflare/ew/pages-infra/-/merge_requests/1167).

## What this repo tests

That `zola build` succeeds on a Cloudflare Pages build **without** setting a `ZOLA_VERSION` environment variable. If Zola is correctly preinstalled, the build passes. If not, it fails with `zola: command not found`.

## Cloudflare Pages settings

| Setting                | Value        |
| ---------------------- | ------------ |
| Build command          | `zola build` |
| Build output directory | `public`     |
| `ZOLA_VERSION`         | *(not set)*  |
