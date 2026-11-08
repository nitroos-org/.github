# Contributing to NitroOS

Thanks for helping! NitroOS is built with AI coding agents under a human maintainer. Please read **[AI-POLICY.md](AI-POLICY.md)** first: it explains how changes are developed, reviewed and tested, and the clean-room rules every patch must follow.

## Quick checklist for a pull request
1. Open it against [nitroos-org/reactos](https://github.com/nitroos-org/reactos) `main`.
2. Sign off every commit: `git commit -s`. This adds `Signed-off-by: Your Name <email>` ([Developer Certificate of Origin](https://developercertificate.org/)). Unsigned commits can't be merged.
3. Licence: the licence of the files you change; new files are GPL-2.0-or-later unless stated otherwise.
4. Say in the description if AI tools helped write the patch. That's fine, just disclose it.
5. Include a reproducer or a test (apitest/winetest/kmtest) for fixes, and what you ran to check it.
6. Clean room: no code or data from leaked or proprietary sources, Microsoft SDK headers, or Microsoft binaries.
7. Behaviour should match Windows. If you rely on a claim about Windows behaviour, say how you know it.

Every patch goes through an independent review and our build and test gates before it lands.

## Reporting bugs
Open an issue with the NitroOS version (`winver`), steps to reproduce, and what you expected. Screenshots and logs help.

## Security
Do **not** open public issues for security problems. See [SECURITY.md](SECURITY.md).
