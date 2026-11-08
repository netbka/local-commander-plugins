# Antivirus and npm malware flags, October–November 2026

*Local Commander (localcmd.app) and Nulio (nulio.ru). Written by Peter Mamrukov, the founder of both products. Last updated 8 November 2026.*

## In short

Between 23 October and 4 November 2026, an antivirus product flagged a small helper file of Local Commander as a trojan, npm removed several versions of our two packages as malware, and both of my npm accounts were blocked.

Since then, three antivirus vendors have reviewed our files independently: Kaspersky called its detection false and fixed it, Bitdefender found our file clean, and AVG whitelisted our files. We also changed the behaviour that looked suspicious.

We are asking npm to review its decision and to restore our accounts. Until that happens, we publish only to our GitLab package registry.

This page keeps the full record in one public place, including what was fair in the flags.

## What our products do

**Local Commander** lets a person's own AI assistant (ChatGPT, Claude and others) read and write files and run commands on that person's own computer. The user installs it, pairs it with their account and sets the permissions. Every action is recorded in an audit log in the user's account.

**Nulio connector** lets a person's own AI assistant work with their own 1C:Enterprise databases on Windows.

Both are dual-use by nature: a tool that runs commands on a computer is exactly what malware does, too. The difference is consent, visibility and control. We think the people who use our tools should know this plainly, so we say it here.

## Timeline

| Date (2026) | What happened |
|---|---|
| 23 Oct | A customer's Kaspersky product flags `JobHost.exe` from Local Commander 0.3.41 as `PDM:Trojan`. |
| 31 Oct | npm removes `local-commander-agent` 0.3.52 as malware. Advisory GHSA-7j62-5j84-72fv / MAL-2026-18425 is published. The same day, the npm account `netbka` stops accepting its password. We open npm support ticket 4932980. |
| 3 Nov | Kaspersky at the same customer flags `JobHost.exe` from 0.3.58 as `PDM:Trojan`. |
| 3–4 Nov | npm removes `nulio-connector` 1.1.10, 1.1.11 and 1.1.16 as malware. Advisory GHSA-v25g-rj4q-4q96 / MAL-2026-18526 is published. The npm account `petermamrukov` is blocked. |
| 4 Nov | We write to npm with full details and ask what was considered malicious. We register in Kaspersky's Allowlist and AVG's Whitelisting programs and submit files to Bitdefender. |
| 4 Nov | Bitdefender's Malware Research Team: `nulio-connector-host.exe` is clean. |
| 4 Nov | We release Local Commander 0.3.64 and Nulio connector 1.1.21 with the changes below, published to GitLab only. |
| 5 Nov | Kaspersky's Allowlist team: the `JobHost.exe` detection "was a false detection. It was fixed." Our allowlist application is with their malware experts. |
| 5–6 Nov | AVG confirms our uploaded Local Commander files are whitelisted. |
| 6 Nov | We send npm the vendor results and ask for a review. |
| 8 Nov | We file a false-positive report with the OpenSSF Malicious Packages project ([ossf/malicious-packages#1756](https://github.com/ossf/malicious-packages/issues/1756)) and ask the community for advice ([community discussion #212435](https://github.com/orgs/community/discussions/212435)). |

## What was fair in the flags, and what we changed

We do not think our software is malicious. But some of what it did looks like malware to automated tools, and we understand why:

**Local Commander (`JobHost.exe`).** It started each command suspended, attached it to a Windows job object, then resumed it, so a whole process tree could be stopped cleanly. Batch files ran through `powershell -EncodedCommand`. Both are common in malware.
In 0.3.64 the process is attached to its job object at creation instead, batch files run through a plain `powershell -Command`, and our helper files carry version information and build reproducibly (the same source always gives identical bytes).

**Nulio connector (background mode, from 1.1.10).** It copied itself into the user's local app data, added a startup entry for the current user, and started without a console window through a small unsigned launcher. That is a classic persistence pattern.
1.1.21 changes this background-mode behaviour.

Neither package had install scripts. Nothing ran on `npm install`.

Our own helper executables are not code-signed yet.

## Files and hashes (SHA-256)

Local Commander 0.3.58, flagged by Kaspersky, later cleared:
- `JobHost.exe` `26e5dac75420637b63221feb1868e19c61d80886ef7f64665ef6d8978902fc67`

Local Commander 0.3.64 (current, GitLab):
- package `7888c44f86ad381d5d8da6dc387cc55f71744efddafd4e9470654689b1b06470`
- `JobHost.exe` `de3e6a4e8a561431c7e696f5267d79625635138482e02c91b9db4936e013b201`
- `LocalCommanderAgent.exe` `375222627daca099b12d9bb888f159570fbb07cecd3d5a1d36940ebd26a0c0a8`
- `OwnerBridge.exe` `98959a63c6f3cfcdaba0b7de146ceb7699caef82392a227df6644d2c502b9127`
- `node.exe` `c22d1c59a1f767a1ed0178445a027f2257d318c55430fc819d48f269586822b7` (official Node.js 22.18.0, signed by the OpenJS Foundation)

Nulio connector 1.1.17, reviewed by Bitdefender:
- `nulio-connector-host.exe` `879876b31acd27ca15d8401fa096d571f42eaf3906be41ec6e240dfde8887eb3`

Nulio connector versions removed by npm (tarball hashes from our release records):
- 1.1.10 `4ea878fcd2b86b5f83c2cfeadbd8f2141f9e9547e71f0d03ee9f46067ff58880`
- 1.1.11 `9c2fe7677cd3c718a94e960c6c283583be2a1fde77aad22e735e8292b13c4140`
- 1.1.16 `8ba4e5ed5825d1470b1db3b41cbf2e82639f4bd8ea8fe774f5b8e06557fa4cf8`

We do not have a copy of the exact bytes npm removed for `local-commander-agent` 0.3.52, and we have asked npm for them.

## Still open

- npm: no answer yet on the classification or the accounts (ticket 4932980).
- GitHub advisories GHSA-7j62-5j84-72fv and GHSA-v25g-rj4q-4q96: GitHub does not accept outside edits to malware advisories, so only npm can withdraw them.
- OpenSSF Malicious Packages: false-positive report [#1756](https://github.com/ossf/malicious-packages/issues/1756) for MAL-2026-18425 and MAL-2026-18526.
- Kaspersky Allowlist: review by their malware experts, usually 12–14 business days.

We will update this page as answers come in.

## Contact

Peter Mamrukov — peter.mamrukov@localcmd.app. If you are a user, a researcher or one of the vendors involved and want the full correspondence, ask me and I will share it.
