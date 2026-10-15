# Exomem status

[Exomem status](https://status.substratesystems.io) reports public checks from outside the application servers.
[Upptime](https://upptime.js.org) runs the checks through GitHub Actions and publishes the site through GitHub Pages.
The repository uses the Upptime 1.46.0 template under its original MIT license.

## Checked services

| Service             | Public endpoint                              | Scope                                                      |
| ------------------- | -------------------------------------------- | ---------------------------------------------------------- |
| Website and sign-in | `https://substratesystems.io/exomem`         | The product website loads.                                 |
| Sign-in service     | `https://substratesystems.io/healthz`        | The sign-in service completes its functional health check. |
| Your Exomem         | `https://exomem.substratesystems.io/healthz` | A canary note can be written and read.                     |
| Sign-in emails      | `https://substratesystems.io/healthz/email`  | The latest synthetic email check is fresh and successful.  |

Every check requires HTTP 200. The three functional health readers reject redirects. Health endpoints must return 200 only when their functional check succeeds and `ok` is `true`.
Upptime 1.46.0 offers text matching, not a structured JSON check, so this configuration relies on that HTTP contract.
A missing health endpoint remains a failed check; the website or API root does not replace it.
Payments link to [Paddle status](https://paddlestatus.com/) and have no local probe.

## Recorded status

No monitoring observations ship with the initial configuration. The first monitoring run creates the status history.
The site shows recorded measurements; the absence of an open incident does not prove that every service is healthy.
GitHub Actions schedules a check every five minutes, but a runner may start late.

<!--start: status pages-->

Monitoring history is unavailable until the first real check completes.

<!--end: status pages-->

## Renderer source

The canonical renderer source lives in `renderer/`. It preserves the routes and build scripts from
[`@upptime/status-page` 1.17.0](https://www.npmjs.com/package/@upptime/status-page/v/1.17.0), published from
[`54c2ff5a3d998d525ee4c7e68dc7ce7414d89c33`](https://github.com/upptime/status-page/tree/54c2ff5a3d998d525ee4c7e68dc7ce7414d89c33).
Its original MIT license remains in `renderer/LICENSE`.
The lockfile comes from that exact upstream revision; DOMPurify 3.4.17 and its type dependency are the only added packages.
Generated configuration, bundles, dependencies, and upstream test suites do not ship as source.

The incident adapter sanitizes Markdown and assembled comment summaries immediately before HTML rendering.
It uses DOMPurify's maintained HTML policy and excludes forms, styles, and page selectors.
Browsers that cannot sanitize show the original content as escaped text.
Service cards show the recorded status as text, with one polite region for status updates.
Their scoped transitions respect reduced motion.
History pages set their document title from the service name.

For an upstream upgrade, compare the published package and its exact Git revision with `renderer/`.
Rebase the incident adapter, status words, motion preference, and history title onto the new source, then update the lockfile.
Run the maintained export and inspect hostile incident content, ordinary Markdown, mixed status words, and unknown-health states.
Check the phone layout, keyboard filter, reduced motion, and forced colors before release.
Rollback by reverting the renderer change to the last reviewed safe revision and deploying “Exomem Pages”.
Do not restore the unsanitized upstream incident renderer as a rollback.

## Operation

The `.upptimerc.yml` file owns the endpoints, schedules, site branding, and contextual secret allowlist.
The `assets/` directory holds the public brand files and dark theme.
Upptime owns these eight generated workflows: `graphs.yml`, `response-time.yml`, `setup.yml`, `site.yml`,
`summary.yml`, `update-template.yml`, `updates.yml`, and `uptime.yml`.
Regenerate them through the maintained template command after a configuration change; do not edit their files.
The maintained template updater replaces only those named workflows, so `exomem-pages.yml` remains separate.

The `skipGeneratingWebsite: true` setting prevents the monitor from installing the upstream public renderer.
The repository keeps “Setup CI” and “Static Site CI” disabled through GitHub workflow settings.
Their files remain intact for template updates; neither workflow may publish the unsanitized renderer.
The other six monitor workflows remain enabled.
“Exomem Pages” builds this source for pull requests and deploys only `master` through GitHub Pages Actions.
It needs no npm account or package publication.

Keep `master` as the default branch: the maintained renderer reads summary and graph files from that branch.
Set the GitHub Pages source to “GitHub Actions”.
Point `status.substratesystems.io` to `substrate-systems.github.io` and verify HTTPS after GitHub provisions the certificate.

The generated workflows try a GitHub App token, then `GH_PAT`, then `GITHUB_TOKEN`.
Start with `GITHUB_TOKEN` and the repository permissions required by the generated workflows.
Add a repository-scoped credential only if an observed operation needs it.
The explicit `secrets: []` setting passes no contextual monitor or notification secrets.
Do not add customer credentials or notification hooks.

1. Disable “Setup CI” and “Static Site CI” through the workflow API; expect both registrations to report `disabled_manually`.
2. Set the Pages source to “GitHub Actions”; expect the repository's Pages configuration to report `workflow`.
3. Run “Update Template CI” after configuration changes; expect all eight generated files and both disabled registrations to remain correct.
4. Run “Uptime CI” after endpoint repairs; expect a real observation for each configured service.
5. Read the latest workflow logs and history timestamps; expect evidence for every reported state.
6. Run “Exomem Pages” from `master`; expect a deployed artifact from the reviewed renderer source.
7. Open the published site at desktop and phone widths; expect current data, working branding, and readable incident details.

Live proof remains pending until the release confirms workflow permissions, real endpoint observations, Pages, DNS, HTTPS, public assets, and incident deep links.
The initial source contains no measurements and makes no claim that production health has been verified.

Incident titles state what people cannot do and what still works.
Close an incident with one sentence that states what happened.
