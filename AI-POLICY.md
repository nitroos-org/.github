# How NitroOS is built: AI policy and patch process

NitroOS is developed with AI coding agents under the direction of a human maintainer. This page says plainly how that works, what we guarantee, and how we accept patches.

## Who does what

- **The maintainer** (Tomas Srnka) decides what gets built. He sets priorities, approves designs, and owns every release. All commits on `main` are made in his name, and he is accountable for them.
- **AI coding agents** do most of the engineering. We use Anthropic's Claude models: **Claude Opus** for the hardest areas (memory management, kernel design, security), **Claude Sonnet** for most implementation and review work, and occasionally **Claude Haiku** for read-only triage. The work is coordinated through Claude Code. The agents handle:
  - investigation, code, tests and documentation;
  - each task is a scoped work item ("lane") with a written brief.
- **No AI agent can publish anything on its own.** Pushing to the public repositories, tagging releases, and uploading downloads are done or approved by the maintainer.

## How a change gets into `main`

1. **Ticket.** A bug, gap or feature is written up, with evidence: a reproducer, a log, or a benchmark.
2. **Work.** An agent works the ticket in an isolated environment. That covers its own build tree and its own virtual machines; it never touches other work in progress.
3. **Independent review.** Every change is reviewed by a *different* model than the one that wrote it (Opus reviews Sonnet's work and Sonnet reviews Opus's). Kernel and security-sensitive changes get a focused review: anything user-controlled is treated as hostile input. Findings go back to the author until the reviewer approves.
4. **Gates before landing:**
   - **Builds:** 32- and 64-bit, Release and Debug, must succeed.
   - **Targeted tests:** the change's own tests, and the existing test suites before and after the change, with no new failures.
   - **Applications:** real applications are used as smoke tests (browsers, development tools, .NET, PowerShell, installers).
5. **After landing:** every landed change is rebuilt from a clean tree.
6. **Before each release:**
   - a full application regression sweep;
   - multi-hour stability soaks;
   - clean builds and boot tests of the installation images;
   - a licence scan.

## Matching Windows, the clean way

- **Behaviour matches Windows by default.** Where Windows behaviour is unclear, we measure it on a reference Windows installation with read-only test programs we write ourselves.
- **We use only public sources:**
  - Microsoft's public documentation;
  - Wine (LGPL);
  - mingw-w64 public headers;
  - Unicode CLDR;
  - the public CCADB certificate reports.
- **We never use:** leaked Microsoft source code, Microsoft SDK headers, Microsoft binaries in our tree, or values copied out of Microsoft programs. Disassembly may be used to locate a problem, never as a source of code or data.
- **We don't copy code from projects whose licence or provenance we can't verify.** That includes other ReactOS derivatives that are not GPL-2.0-compatible.
- **New binary files** (fonts, data tables, images) must have a documented origin. Generated data comes with the script that generates it.

## Our relationship to ReactOS

- NitroOS is an **downstream** of ReactOS. It is not affiliated with or endorsed by the ReactOS project.
- We track upstream ReactOS closely and keep our changes as a patch set on top of it.
- ReactOS does not accept AI-generated contributions. We respect that: **we do not submit AI-developed changes upstream.** Our source is public under ReactOS's licences, as the GPL requires.

## Contributing patches

**We welcome patches.** Open a pull request on [nitroos-org/reactos](https://github.com/nitroos-org/reactos).

- **Licence:** contributions are accepted under the licence of the files they change, GPL-2.0-or-later for new files unless stated otherwise.
- **Sign-off required (Developer Certificate of Origin):** every commit must carry a `Signed-off-by: Your Name <email>` line (`git commit -s`). It certifies that you wrote the patch, or otherwise have the right to submit it under the project's licence, as described at https://developercertificate.org/.
- **AI-assisted patches are welcome; disclose it** in the pull request. We hold them to the same bar as everything else.
- **Clean-room rules apply to you too.** Do not submit code derived from leaked or proprietary sources.
- **Same gates as our own work:**
  - an independent review;
  - the build and test gates above;
  - an application smoke test where relevant.

  Please include a reproducer or test with fixes.
- **Security issues:** report them privately. Do not use public issues. Contact: **security@nitroos.org**.

## What you can rely on

- **Every change on `main`** was reviewed by a second model and passed automated builds and tests.
- **Every release** passed a full application sweep, stability soaks and boot tests.
- **Known limitations are listed openly** in each release's notes.
- **Benchmarks and compatibility claims** on our pages are backed by measurements or screenshots we can show.

