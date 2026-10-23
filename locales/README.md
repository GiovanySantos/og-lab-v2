# **DO NOT edit locale files** except `ja-JP.yml`.

In upstream Misskey, text added to `ja-JP.yml` is distributed to other
languages through its Crowdin workflow. OG Lab is a fork and does not have
access to or contact with the upstream Crowdin administrators, so do not assume
that this synchronization will publish OG Lab translations.
Translations added in ja-JP file should contain the original Japanese strings.

Please see [Contribution guide](../CONTRIBUTING.md) for more information.

## OG Lab fork language strategy

OG Lab targets Brazilian Portuguese only and is gradually removing Crowdin and
runtime i18n dependencies. Migrated UI surfaces should use direct Brazilian
Portuguese text in the frontend instead of adding locale keys.

Keep the shared localization runtime, locale files, and Crowdin configuration
until all unmigrated application surfaces no longer depend on them. Do not
remove the global infrastructure as part of a partial migration.

This fork has no access to or contact with upstream Misskey Crowdin
administrators. Never make migration work depend on upstream permissions,
project changes, or administrator actions. Do not manually edit non-source
locale YAML files.
