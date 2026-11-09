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
| 2026-11-08 | 99 | 4996–5094 | `aca3548575e1480d` | [2026-11-08.json](roots/2026-11-08.json) |
| 2026-11-07 | 99 | 4897–4995 | `a13a5e7877a71a03` | [2026-11-07.json](roots/2026-11-07.json) |
| 2026-11-06 | 99 | 4798–4896 | `ce8f1e07f1a5d519` | [2026-11-06.json](roots/2026-11-06.json) |
| 2026-11-05 | 99 | 4699–4797 | `5ff2697ac6234b7d` | [2026-11-05.json](roots/2026-11-05.json) |
| 2026-11-04 | 99 | 4600–4698 | `efa7b17e13d21eee` | [2026-11-04.json](roots/2026-11-04.json) |
| 2026-11-03 | 99 | 4501–4599 | `79f9612c37eeda03` | [2026-11-03.json](roots/2026-11-03.json) |
| 2026-11-02 | 99 | 4402–4500 | `b06f2cacf9b38b7b` | [2026-11-02.json](roots/2026-11-02.json) |
| 2026-11-01 | 99 | 4303–4401 | `e733c058e192ae2b` | [2026-11-01.json](roots/2026-11-01.json) |
| 2026-10-31 | 99 | 4204–4302 | `75de4bacbd9e80c0` | [2026-10-31.json](roots/2026-10-31.json) |
| 2026-10-30 | 99 | 4105–4203 | `f1873f38e20dcaa5` | [2026-10-30.json](roots/2026-10-30.json) |
| 2026-10-29 | 99 | 4006–4104 | `4d03fdd9b1a08fcc` | [2026-10-29.json](roots/2026-10-29.json) |
| 2026-10-28 | 99 | 3907–4005 | `b319217d92e5c014` | [2026-10-28.json](roots/2026-10-28.json) |
| 2026-10-27 | 99 | 3808–3906 | `e4bf2329401ca55c` | [2026-10-27.json](roots/2026-10-27.json) |
| 2026-10-26 | 99 | 3709–3807 | `301aa6ace3f42443` | [2026-10-26.json](roots/2026-10-26.json) |
| 2026-10-25 | 99 | 3610–3708 | `e9b0f27bf1c969d4` | [2026-10-25.json](roots/2026-10-25.json) |
| 2026-10-24 | 99 | 3511–3609 | `96779432597a7a5d` | [2026-10-24.json](roots/2026-10-24.json) |
| 2026-10-23 | 99 | 3412–3510 | `fae41686bfd34630` | [2026-10-23.json](roots/2026-10-23.json) |
| 2026-10-22 | 99 | 3313–3411 | `cf811580f51c59ff` | [2026-10-22.json](roots/2026-10-22.json) |
| 2026-10-21 | 99 | 3214–3312 | `793d818db6e3b81c` | [2026-10-21.json](roots/2026-10-21.json) |
| 2026-10-20 | 99 | 3115–3213 | `71317eecdafebc27` | [2026-10-20.json](roots/2026-10-20.json) |
| 2026-10-19 | 99 | 3016–3114 | `b5e461c27681e001` | [2026-10-19.json](roots/2026-10-19.json) |
| 2026-10-18 | 99 | 2917–3015 | `0e5d79f4567ff96e` | [2026-10-18.json](roots/2026-10-18.json) |
| 2026-10-17 | 99 | 2818–2916 | `92018cb31c9bcf11` | [2026-10-17.json](roots/2026-10-17.json) |
| 2026-10-16 | 99 | 2719–2817 | `c2b118adc5a44bd4` | [2026-10-16.json](roots/2026-10-16.json) |
| 2026-10-15 | 99 | 2620–2718 | `a50405f5849fbeee` | [2026-10-15.json](roots/2026-10-15.json) |
| 2026-10-14 | 99 | 2521–2619 | `bbab6fa379f53456` | [2026-10-14.json](roots/2026-10-14.json) |
| 2026-10-13 | 99 | 2422–2520 | `dbb638ea04a124c5` | [2026-10-13.json](roots/2026-10-13.json) |
| 2026-10-12 | 49 | 2373–2421 | `aea57954b0d8ac5a` | [2026-10-12.json](roots/2026-10-12.json) |
| 2026-10-11 | 36 | 2337–2372 | `5adfe0030b992147` | [2026-10-11.json](roots/2026-10-11.json) |
| 2026-10-10 | 36 | 2301–2336 | `05f634b3ec61e33a` | [2026-10-10.json](roots/2026-10-10.json) |
| … | | | | 37 older file(s) in `roots/` |
<!-- roots-table:end -->
