# Instance configuration, access, and operations

**Status:** Pending

## Scope

Inventory instance-level settings and application surfaces related to OG Lab's
public identity, account access, and federation behavior. The intended access
policy and whether federation remains enabled have not yet been specified.

## Locations to review

- `packages/frontend/src/pages/admin/settings.vue` — administrative federation
  controls and related instance settings.
- `packages/frontend/src/pages/admin/moderation.vue` and
  `packages/frontend/src/pages/admin/invites.vue` — registration and invitation
  management surfaces.
- `packages/backend/src/models/Meta.ts` — persisted instance registration,
  federation, and identity fields.
- `packages/backend/src/server/api/endpoints/admin/update-meta.ts` and
  `packages/backend/src/server/api/endpoints/admin/meta.ts` — administrative
  settings update and metadata API.
- `packages/backend/src/server/api/SignupApiService.ts` — signup and
  registration-ticket handling.
- `packages/backend/src/core/UtilityService.ts` — federation host eligibility.
- `packages/backend/src/server/ActivityPubServerService.ts`,
  `packages/backend/src/server/WellKnownServerService.ts`, and
  `packages/backend/src/server/NodeinfoServerService.ts` — inbound federation,
  discovery, and instance capability reporting.
- `packages/backend/src/core/entities/MetaEntityService.ts` and
  `packages/backend/src/server/web/HtmlTemplateService.ts` — instance metadata
  exposed in responses and rendered pages.
- Unauthenticated routes and assets across `packages/backend/src/server/`,
  including web routes, API endpoints, media delivery, feeds, and generated
  public metadata.
- Backend tests under `packages/backend/test/` and frontend tests under
  `packages/frontend/` that cover the affected settings, routes, and visible
  access behavior.

## Questions to resolve during the work

- Whether access is public, login-only, or limited to an authorized subset of
  accounts.
- Whether ActivityPub federation remains enabled, is limited, or is disabled.
- How account creation and administration are handled.
- What visibility applies to existing remote accounts and content.
- Which public routes, metadata, and media remain available without login.
