# jitsi-pr-tests-pages

Per-PR test report hosting for Jitsi CI. Served via GitHub Pages at
<https://jitsi.github.io/jitsi-pr-tests-pages/>.

This repository **only stores generated CI artifacts**. Do not edit branches
other than `main` (landing page sources) by hand. The `gh-pages` branch is
machine-managed by the Jitsi PR test pipeline.

## URL layout

Reports are pushed by `publish-pr-results.sh` (from
[`jitsi-pr-testing-scripts`](https://github.com/jitsi/jitsi-pr-testing-scripts))
under this structure on the `gh-pages` branch:

```
<source-repo>/PR-<pr-number>/<build-number>/
  index.html              # landing page with links
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
