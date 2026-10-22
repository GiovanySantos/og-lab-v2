# Platform visual identity and user-facing text

**Status:** Pending

## Scope

Inventory and identify the interface locations and static assets that display
the product identity, instance identity, or default public-facing description.

## Locations to review

- `packages/frontend/assets/misskey.svg` — logo used by the welcome entrance
  screens and QR display.
- `packages/frontend/src/pages/welcome.entrance.simple.vue` and
  `packages/frontend/src/pages/welcome.entrance.classic.vue` — welcome screen
  branding.
- `packages/frontend/src/pages/qr.show.vue` — QR page branding.
- `packages/frontend/src/pages/about-misskey.vue` — product information and
  upstream-oriented about page.
- `packages/frontend/src/pages/welcome.vue`,
  `packages/frontend/src/pages/about.vue`, and
  `packages/frontend/src/pages/about.overview.vue` — instance introduction
  and public instance information.
- `packages/frontend/src/pages/admin/branding.vue` — administrative branding
  controls.
- `packages/frontend/src/components/MkVisitorDashboard.vue`,
  `packages/frontend/src/components/MkSignin.vue`, and
  `packages/frontend/src/components/MkSignupDialog.vue` — visitor, sign-in,
  and sign-up surfaces.
- `packages/frontend/assets/about-icon.png` — icon used on the product
  information page.
- `packages/backend/assets/favicon.png`, `favicon.ico`,
  `apple-touch-icon.png`, `icons/192.png`, and `icons/512.png` — browser and
  installed-app icons.
- `packages/backend/src/server/web/ClientServerService.ts` — serving browser
  icon assets.
- `packages/backend/src/server/web/HtmlTemplateService.ts` and
  `packages/backend/src/server/web/views/_.ts` — instance name, icon, theme
  color, and generated HTML metadata.
- `locales/ja-JP.yml` — source strings for the Japanese interface, including
  product and instance-facing copy. Other locale YAML files are synchronized
  and are not manual editing targets.

## Notes

This item is an inventory of locations, not a specification of replacement
artwork, colors, wording, or product policy.
