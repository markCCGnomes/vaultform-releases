<p align="center">
  <img src="brand/vaultform.svg" width="120" alt="VaultForm">
</p>

# VaultForm — official releases

**VaultForm** is a privacy-first form submission data store: a
[Holochain](https://holochain.org) app that keeps form submissions on
infrastructure **you** own — your own device or your always-on
[Vault Node](https://github.com/markCCGnomes/vault-node-releases) — instead of a
third-party cloud. It is the storage backend for the
**DataFerry** WordPress form builder: submissions, attachments, and PDF receipts
flow from your website straight to your own node. Submitters receive a private
claim token letting them retrieve their own submission at any time — no account
needed.

This repository hosts the **official release artifacts** (source is maintained
privately by CCGnomes):

| Asset | What it is |
|---|---|
| `vaultform.happ` | The Holochain app bundle every device and Vault Node installs |
| `vault-node.manifest.json` | Lockstep manifest a Vault Node verifies before install |
| `vaultform-http-api-x86_64-linux` | The HTTP API sidecar (port 2341) that DataFerry talks to |

## Install

The easy path is a Vault Node: flash a
[Vault Node image](https://github.com/markCCGnomes/vault-node-releases), then
enable VaultForm on it — the node fetches and verifies the artifacts above
automatically. Point DataFerry's VaultForm endpoint at your node and you're done.

Manual/advanced setups: see each release's notes. All artifacts on a given
release tag are **lockstep** — install the `.happ` and the sidecar binary from
the *same tag*, never mix versions.

## Verify your download

Each release's `vault-node.manifest.json` contains the SHA-256 of the exact
`.happ` bytes (`happ_sha256`). Vault Nodes check this automatically; manual
installs can too:

```bash
sha256sum vaultform.happ   # must match happ_sha256 in vault-node.manifest.json
```

## License & donations

Free for personal **and** commercial use, self-hosted. Not for redistribution,
modification, or offering as a hosted service to third parties — see
[LICENSE](LICENSE). VaultForm is community-funded: if it's useful to you,
donations toward continued development are warmly encouraged at
**https://ccgnomes.com/donate**.

## Security

Please report vulnerabilities privately via this repository's
"Report a vulnerability" button (GitHub private vulnerability reporting) rather
than a public issue.

---

<p align="center">
  <img src="brand/vaultsuite-icon.png" width="72" alt="VaultSuite — every tool, one vault">
  <br>
  <sub>VaultForm is part of the <b>VaultSuite</b> family of self-hosted, privacy-first tools by CCGnomes.</sub>
</p>
