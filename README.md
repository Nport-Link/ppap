# ppap

**This repository runs NPort's CI. It does not hold NPort's source.**

`tuanngocptn/nport-link` is private, and private repositories meter GitHub Actions minutes. This one
is public, so its GitHub-hosted minutes are unlimited. The jobs therefore live here: they check the
private tree out with a read-only token, run, and report the verdict back as commit statuses named
`ppap / …` on the commit that triggered them.

## Do not edit `.github/workflows/` here

Every file in it is **generated**. The source of truth is `.github/workflows/ppap/` in
`tuanngocptn/nport-link`, and `sync-ppap.yml` mirrors that directory here with `rsync --delete` on
every push to `main`. An edit made in this repository is overwritten without warning the next time
anything upstream changes — including a file you add, which the `--delete` removes.

Change the workflows there, not here.

## How a run starts

Nothing here watches the private repository. A push to it runs `trigger-ppap.yml`, which sends a
`repository_dispatch` of type `nport-ci` carrying the commit SHA; `ci.yml` and `codegen-drift.yml`
both subscribe to it. `smoke.yml` is the exception — it is a nightly cron and takes no dispatch.

There is deliberately **no `pull_request` trigger** on anything here. A pull request from a fork
would otherwise be able to reach this repository's Actions cache, which holds compiled artifacts of
private source. Do not add one.

## Logs here are public

That is understood and accepted upstream, not an oversight — but it is worth knowing before reading
a failure: a red run prints source spans from a private codebase into a world-readable log.

## Where the documentation is

`docs/CI.md` in the private repository has the topology, the token permissions, the self-hosted
runner notes, and a table of failure modes. ADR-0057 has the reasoning and the rejected alternatives.
