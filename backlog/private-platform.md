# Make the platform private and login-only

**Status:** Pending

## Goal

Operate OG Lab as a private social network where only authenticated, authorized
accounts can access platform content and functionality, with no ActivityPub
federation.

## Scope to investigate

- Disable federation using the existing server setting and verify all inbound
  and outbound ActivityPub routes honor it.
- Disable public registration and define the account-provisioning process
  (such as invitations or administrator-created accounts).
- Audit unauthenticated web pages, API endpoints, media files, previews,
  metadata, and other public access paths.
- Decide whether content from remote accounts already stored in the database
  should be retained or removed.
- Add automated tests for access control and federation behavior.

## Acceptance criteria

- Unauthenticated visitors cannot read posts, profiles, media, or other
  instance content through the web UI or APIs.
- Only accounts provisioned under the selected policy can sign in.
- The instance neither sends federation activities nor accepts remote
  federation traffic.
- Existing data handling and operational limitations are documented.
- Tests cover authenticated and unauthenticated access, plus federation being
  disabled.
