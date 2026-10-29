# jitsi-pr-tests-pages

Per-PR test report hosting for Jitsi CI. Served via GitHub Pages at
<https://jitsi.github.io/jitsi-pr-tests-pages/>.

This repository **only stores generated CI artifacts**. Do not edit branches
other than `main` (landing page sources) by hand. The `gh-pages` branch is
machine-managed by the Jitsi PR test pipeline.

## Pages on `main`

`index.html` (the landing page) and `queue/index.html` are written by hand on `main`.
`.github/workflows/publish-landing.yml` copies them to `gh-pages` on every push to `main`
that changes them.

The queue page, <https://jitsi.github.io/jitsi-pr-tests-pages/queue/>, lists the PR test runs
of jitsi-meet, lib-jitsi-meet, jicofo, jitsi-videobridge and jicoco that are waiting or running.
It reads the public GitHub API from the browser, without a token, and lists the runs
that finished in the last 48 hours with the result from each build's `result.json`.

## URL layout

Reports are pushed by `publish-pr-results.sh` (from
[`jitsi-pr-testing-scripts`](https://github.com/jitsi/jitsi-pr-testing-scripts))
under this structure on the `gh-pages` branch:

```
<source-repo>/PR-<pr-number>/<build-number>/
  index.html              # landing page with links
  result.json             # the result, as in the PR check (read by the queue page)
  test-reports.zip        # full archive of the run
  log.txt                 # build log
  chrome-chrome/          # Allure HTML report (Chrome / Chrome)
    index.html
    ...
  ff-chrome/              # Allure HTML report (Firefox / Chrome)
    index.html
    ...
```

Example: <https://jitsi.github.io/jitsi-pr-tests-pages/jitsi-meet/PR-17436/20120/>

## Retention

`.github/workflows/prune.yml` runs daily and deletes any
`<repo>/PR-<n>/<build>/` directory older than 60 days. Adjust
`RETENTION_DAYS` there if you need to keep more or less.

## Who pushes here

A GitHub App `jitsi-ci` (installation token) is the only writer. The token
needs `contents: write` on this repo. No human pushes to `gh-pages`.
