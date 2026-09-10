# EXHIBIT B — daily roots

Every day at 00:05 UTC, the receipts sealed the previous day are hashed into one Merkle root and
that root is committed here. Each file is signed and carries the previous day's root, so the files
form a chain of their own: a root published on the 4th cannot be quietly rewritten on the 9th
without breaking every file after it.

This repository exists so that the record does not live only in our database. If our service
disappears, a receipt you hold still verifies against these files, with our open-source verifier,
offline.

```
roots/
  2026-09-04.json      one signed root per UTC day
  latest.json          a copy of the most recent root
keys.json              the public keys receipts and roots are signed with
```

## Verifying a receipt against a root

```bash
npx exhibitb verify receipt.json --proof proof.json --root roots/2026-09-04.json --keys keys.json
```

`proof.json` comes from the issuer (`/api/receipt/<id>/proof`) and lists the audit path from your
receipt's hash to the day's root. The verifier recomputes the root from your receipt alone and
compares it with the signed file here. Nothing in this repository reveals any document, quotation,
or client material: a root is a hash of hashes.

## Root file format

```jsonc
{
  "v": 1,
  "kind": "exhibitb.root",
  "date": "2026-09-04",
  "first_seq": 1801,          // first receipt sealed that day (null on an empty day)
  "last_seq": 1899,
  "count": 99,
  "root": "<hex sha256>",     // RFC 6962 Merkle root over the day's receipt hashes, in seq order
  "prev_root": "<hex sha256>",
  "head_hash": "<hex sha256>", // hash of the last receipt in the chain at publication time
  "key_id": "eb-root-2026-09",
  "self_hash": "<hex sha256>",
  "signatures": [{ "key_id": "eb-root-2026-09", "alg": "ed25519", "sig": "<base64>" }]
}
```

The canonical form the hashes are taken over, the leaf and node rules, and the verifier's check
order are specified in `spec/RECEIPT.md` of the verifier repository.

## What a root proves, and what it does not

A root proves that a receipt existed, unchanged, on the day the root was published. It says nothing
about whether a cited source is any good, and nothing about the conclusions of the report the
receipt belongs to.

*Attests what was checked, against which sources, at what time. Not a claim of truth.*

## Published roots

Newest first. Every file is signed and carries the previous day's root.

<!-- roots-table:start -->
| Date | Receipts | Seq | Root (sha256, first 16) | File |
|---|---|---|---|---|
| 2026-09-09 | 12 | 22–33 | `c892ebafd1b9d414` | [2026-09-09.json](roots/2026-09-09.json) |
| 2026-09-08 | 19 | 3–21 | `e761abd46adc0495` | [2026-09-08.json](roots/2026-09-08.json) |
| 2026-09-07 | 1 | 2–2 | `e25f7e1f827753f2` | [2026-09-07.json](roots/2026-09-07.json) |
| 2026-09-06 | 1 | 1–1 | `9c56172ff2902b93` | [2026-09-06.json](roots/2026-09-06.json) |
| 2026-09-05 | 0 | — | `e3b0c44298fc1c14` | [2026-09-05.json](roots/2026-09-05.json) |
| 2026-09-04 | 0 | — | `e3b0c44298fc1c14` | [2026-09-04.json](roots/2026-09-04.json) |
| 2026-09-03 | 0 | — | `e3b0c44298fc1c14` | [2026-09-03.json](roots/2026-09-03.json) |
<!-- roots-table:end -->
