# Git Operations Log

This file records the git operations performed on this project — repository setup, remotes, deploy-triggering pushes, and the full commit history. It is generated from `git log` and `git remote -v`, not hand-maintained; regenerate it if it drifts.

## Repositories

| Remote | URL | Visibility | Role |
|---|---|---|---|
| `origin` | https://github.com/Rameshwarnaik013/farmley-dispatch-plan | Public | Live — Vercel auto-deploys on every push to `master` |
| `farmley-logistics` | https://github.com/jatothr-lgtm/Farmley-Logistics | Private | Mirror — created 2026-08-21, receives every push alongside `origin` |

Both remotes track the same `master` branch and are kept in sync commit-for-commit; every push in this log went to both unless noted otherwise.

## Deploy mechanics

- `vercel.json`: `outputDirectory: public`, no build step, `Cache-Control: no-cache, must-revalidate` on every response so a fresh deploy is visible immediately rather than sitting behind a browser cache.
- The header badge in the running app (`FARMLEY LOGISTICS · v<date>.<n>`) is bumped on every functional change — check it against this log to confirm which commit a browser is actually running.
- `dispatch_plan_generator.html` (repo root) is kept byte-identical to `public/index.html` on every commit that touches the app; only `public/` is what Vercel serves.

## Notable operations (not visible from the commit list alone)

- **2026-08-21** — Created `jatothr-lgtm/Farmley-Logistics` (private) as a second remote, `git remote add farmley-logistics`, and pushed the full history to it (`git push -u farmley-logistics master`). Chosen private because the codebase embeds real carrier commercial terms (minimum weights, docket fees, ODA slabs) and, from `8605e38` onward, a 3.5 MB `masters.json` containing the full pincode/rate/customer master.
- **2026-08-22** — Two pushes to `origin` (`eed8f5a→…`, then later `d3b0ecd→…`) were blocked by the local session's permission layer and only completed after explicit user authorization to proceed.
- **2026-08-22** — `af99953` is a `git revert --no-commit` of `cee23f9` followed by a commit, restoring the tree to the exact state of `cbda121` byte-for-byte (`git diff cbda121 HEAD` empty) at the user's request, without editing any code by hand.
- Commit authorship is fixed at `Rameshwarnaik013 <rameshwarnaik08@gmail.com>` project-wide — required for Vercel's Hobby-plan deploy check regardless of which GitHub account is pushing.

## Full commit history

`master`, oldest first, 51 commits:

| Commit | Date | Message |
|---|---|---|
| 3ca6563 | 2026-05-17 11:11 | Dispatch Plan Generator with Excel formula-based export |
| 8305bab | 2026-05-18 12:19 | Airlines: use Delivery_Date as po_expiry_date |
| 18e1911 | 2026-05-18 17:49 | Remove Airlines po_expiry_date = Delivery_Date condition |
| 5912692 | 2026-05-20 11:22 | Remove Condition 21: Remark closed override |
| 91e0496 | 2026-05-20 12:09 | Add Condition 23: Munchies Waves + Tamil Nadu → Origin Rebela |
| 27bf3f9 | 2026-05-27 15:32 | Add Condition 24: Airlines customer_group uses Delivery_Date as po_expiry_date |
| d1c6163 | 2026-06-03 18:20 | Add Conditions 25-27: multi-date Appt, Rea lookup, CFA Bangalore |
| 3e61305 | 2026-06-05 13:53 | Expand CFA Bangalore Item_Code list with 28 additional Makhana SKUs |
| 70ede23 | 2026-06-09 17:03 | Fix FK Appointments lookup: detect all PO columns, handle multi-date as-is |
| 94b9762 | 2026-06-29 15:03 | Keep free-text Remarks (e.g. "FTL 28 Jun Appt") verbatim instead of coercing to a date |
| a1f22b0 | 2026-07-10 15:32 | Update dispatch_plan_generator.html |
| d877991 | 2026-07-10 15:38 | Rename origin labels: CFA (Gurgaon) -> CFA (GGN), CFA Bangalore -> CFA (BLR) |
| 316a749 | 2026-07-10 15:48 | Sync root dispatch_plan_generator.html to deployed public/index.html |
| 087a1e7 | 2026-07-20 13:08 | Condition 27: rebuild CFA (BLR) rule on Item_Name + Shipping State + stock |
| f8f6e74 | 2026-07-20 16:39 | Condition 27: require customer_group for CFA (BLR) |
| 92b51b6 | 2026-07-22 15:14 | Condition 28: Quick Commerce / E-Commerce Branch -> Origin map |
| bf902c0 | 2026-07-22 18:28 | Drop stock check + stock file; CFA rules to 3 checks; Cond 28 = Quick Commerce only |
| c7f8117 | 2026-07-24 15:55 | Is CFA = computed Yes/No; Rea becomes origin-validation flag (Cond 26 lookup cancelled) |
| a4f87a6 | 2026-07-24 19:07 | Rea validation: add Munchies Waves-Airlines->Rebela, Dry Fruit Combo & Gifting->Indore |
| d18a3f0 | 2026-07-24 19:16 | Use one common 75-item CFA SKU list for both CFA (GGN) and CFA (BLR) |
| ea46bd8 | 2026-07-24 19:31 | Cancel Condition 15: Priority column stays in output but is always empty |
| 79a4a73 | 2026-07-24 19:34 | Add Assorted Pack Coffee variant to the common CFA SKU list (75 -> 76 items) |
| 5be0c16 | 2026-07-24 19:43 | Priority column now carries the reason when Rea is FALSE |
| 3c4752d | 2026-07-24 19:53 | Add no-cache header + visible build tag (Priority/Rea logic verified working) |
| 62eac2d | 2026-07-25 13:54 | Make LEAD TIME, Appointments and Yesterday inputs optional (Dump stays required) |
| 963e869 | 2026-07-27 12:49 | Validate CFA items by Item_Code instead of Item_Name (75 codes) |
| d34dae0 | 2026-07-28 11:49 | Add Item_Code Dates_4-20183 to the common CFA list (75 -> 76 codes) |
| 4a5cc87 | 2026-07-28 18:56 | Condition 30: add CFA Serviceability Check column |
| 8dc2da5 | 2026-07-28 19:23 | Rebase Is CFA on CFA serviceability; add Expected Source column |
| e387f0d | 2026-07-29 12:09 | Condition 31: derive Expected Source from Item_Group / item_parent |
| 5aa4e48 | 2026-07-29 12:33 | Standardise CFA labels: drop parentheses, one spelling everywhere |
| 87bc73a | 2026-07-29 12:54 | Revert CFA label rename on Origin; only Expected Source uses CFA GGN / CFA BLR |
| 03b7055 | 2026-07-29 14:12 | Expected Source by customer_group; add Order Corrected Remark column |
| 3ed4db2 | 2026-07-29 15:17 | Fill Order Corrected Remark blanks; add Source Check column |
| 5a68f7c | 2026-07-29 16:25 | Normalise CFA labels in Expected Source and CFA Serviceability Check |
| 1bc776f | 2026-07-29 17:01 | Source Check now compares Expected Source against Origin |
| 67d680d | 2026-07-29 18:06 | Source Check: require three-way agreement |
| fe6abaf | 2026-07-30 12:24 | Add Analysis tab with 5 summary cross-tabs to the exported Excel |
| f520038 | 2026-07-30 12:36 | Add Pivot Source tab for building real Excel PivotTables |
| b168b5b | 2026-07-30 12:54 | Split Analysis into 5 filterable pivot sheets, promote CFA tags, add color-coded emoji markers |
| c6957b7 | 2026-07-30 13:07 | Add real AutoFilter to Pivot Source for Source Check / customer_group filtering |
| ea8a6c0 | 2026-08-07 13:06 | Update common CFA SKU list to 80 codes from ITEM NAME.xlsx (76 -> 80) |
| 30409d8 | 2026-08-07 13:36 | Fetch text values (e.g. Cancelled) into Appt, not just dates |
| 4da0e82 | 2026-08-20 12:19 | Remove 3 Item_Codes from the common CFA list (80 -> 77) |
| 0ed261f | 2026-08-21 18:09 | Add Masters editor and Lowest 1/2 freight ranking |
| 8605e38 | 2026-08-21 18:14 | Ship the master data with the app |
| eed8f5a | 2026-08-21 18:23 | Stop a catch-all rewrite from silently swallowing masters.json |
| d3b0ecd | 2026-08-22 18:23 | Do not force-cache the bundled master |
| cbda121 | 2026-08-22 18:28 | Stop an empty stored master from silently blanking Lowest 1/2 |
| cee23f9 | 2026-08-22 18:58 | Fill the master data gaps found against the live plan |
| af99953 | 2026-08-22 19:04 | Revert "Fill the master data gaps found against the live plan" |
| 4166c05 | 2026-08-24 11:43 | Rebuild the logistic master from raw carrier tabs |
---
*Regenerate the table above with:*
```bash
git log --pretty=format:"| %h | %ad | %s |" --date=format:"%Y-%m-%d %H:%M" --reverse
```
