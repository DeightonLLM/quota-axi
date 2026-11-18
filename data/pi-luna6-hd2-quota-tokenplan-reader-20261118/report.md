# Qwen Token Plan quota reader — evidence report

## Ownership and authorized repository

The vendor origin is `https://github.com/kunchenguid/quota-axi.git` (README identifies `@kunchenguid`; `LICENSE` names Kun Chen). Per Captain-authorized decision `hd2-quota-axi-delivery-20261118`, the authorized repository is the organization copy `https://github.com/DeightonLLM/quota-axi.git` (`dell`). The vendor origin was not fetched from, contacted, or modified.

The isolated worktree is `/root/.treehouse/quota-axi-35f98d/1/quota-axi`. Branch `fm/pi-luna6-hd2-quota-tokenplan-reader-20261118` is based on `dell/main` at `e565fe0c8f21df63bd30eb7d179da4c5363e7c7b`. The supplied vendor snapshot is `5734c41eee52eb9a80ba67f47269ebc06c0c41fc`; it is 105 commits ahead of `dell/main` (`git rev-list --left-right --count vendor-snapshot...dell/main` => `105 0`). The worktree follows the authorized org baseline; no vendor changes were overlaid.

## Official-source checks

All requests below were unauthenticated, read-only GETs to official QwenCloud documentation/console or static assets. No API key, cookie, or other credential was sent.

- `https://docs.qwencloud.com/token-plan/overview` — HTTP 200. Describes Token Plan as monthly Credits quota, links to the Token Plan Usage and quota documentation and console pages, and documents dedicated model-inference endpoints/API keys.
- `https://docs.qwencloud.com/token-plan/token-plan-usage-quota` — HTTP 200. Describes quota deduction, subscription-month reset, and usage troubleshooting. It does not document a machine-readable usage/quota API endpoint or response schema.
- `https://docs.qwencloud.com/token-plan/personal/token-plan-personal-overview` — HTTP 200. Describes monthly plan Credits and the console's usage details; it does not specify a usage API endpoint/schema.
- `https://docs.qwencloud.com/token-plan/team/token-plan-team-overview` — HTTP 200. Describes per-seat monthly quota and says the console can show member-level consumption; it does not specify an API endpoint/schema.
- `https://docs.qwencloud.com/token-plan/token-plan-permissions-and-usage` and `/token-plan/token-plan-faq` — HTTP 200. Reviewed for quota/access guidance; no public usage API callable with the model credential is documented.
- `https://home.qwencloud.com/analytics/token-plan` — HTTP 200, returned the generic QwenCloud console shell. The official console frontend bundle includes the message “You are currently not logged in. Log in to access all services.” Its usage analytics calls are console `ModelStudio` actions (including `GetTokenPlanUsageTrend`, `ListTokenPlanUsageRecords`/`ListTokenPlanUsageStatistics`, and `ListTokenPlanUsageDimensionValues`), not a documented API accepting the Token Plan model API key. No login or dashboard session was attempted.
- Static Qwen Code bundle (`/usr/lib/node_modules/@qwen-code/qwen-code/chunks/chunk-D7FTEPLO.js`) documents the Token Plan model-compatible base URLs `https://token-plan.maas.qwencloudapi.com/compatible-mode/v1` and `https://token-plan.maas.qianwenaiapi.com/compatible-mode/v1`; it identifies the Token Plan API key as model-provider configuration. The installed Qwen CLI contains no quota-reader action or entitlement/usage file schema.

Local configuration was inspected without printing values. `~/.qwen/settings.json` contains provider metadata keys `qwencloud-standard` and `qwencloud-token-plan` and environment-variable names `QWENCLOUD_API_KEY` and `QWENCLOUD_TOKEN_PLAN_API_KEY`; these are model-provider settings, not quota measurements. `~/.qwen/subscription_credentials.json` has fields `baseUrl`, `clientId`, `revision`, and `signInAttempt`; its inspected schema has no quota/usage/entitlement fields. No credential values were recorded or emitted.

## Outcome and limits

The authoritative public documentation describes quota display and usage analytics in the authenticated QwenCloud console, while the existing harness credential is a Token Plan model API key for model requests. No public quota endpoint/schema callable with that credential, nor a local numeric entitlement record, was found in the checked sources. The dashboard's console-only usage actions are not used as an undocumented API, and no credential-bearing request was made. quota-axi cannot truthfully instrument Qwen Token Plan until Qwen exposes or documents a supported usage/quota API (or a supported local entitlement record).

No source or test files were changed; no Qwen quota line can be demonstrated and current no-quota/unknown output remains appropriate. No tests or lint were run because there is no supported interface to implement or fixture contract to test. The earlier ownership check found the organization main baseline is behind the supplied vendor snapshot as described above; no package behavior from the vendor-only delta was assumed.

**VERDICT: NOT_INSTRUMENTABLE_UPSTREAM_API — Qwen's checked official documentation/console exposes usage to its authenticated web console, but no documented quota endpoint callable with the existing harness model credential or local quota record was found.**
