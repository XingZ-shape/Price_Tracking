# Deployment Acceptance and Regression Scenarios

This file specifies execution conditions and expected checks; it does not claim any site has been tested. For a new site/tool environment, first pass a genuinely authorized preflight. Tests must not place orders, claim coupons, change quantities, or alter production history.

## Required acceptance checks
- Read the official cart; refresh successfully and wait for stable loading. Obtain UI evidence of login and, where applicable, an explicitly empty cart.
- Have evidence of scrolling to the bottom, expanding unavailable-item sections, and coverage counts—not just the first screen.
- Establish identity sources. Distinguish VERIFIED_SKU / VERIFIED_SINGLE_VARIANT / UNRESOLVED_VARIANT; neither discard rows nor invent IDs for missing SKUs.
- For authorized selectable rows, verify single-row-only selection, unchanged original quantity, stable quotes, and evidence-backed restoration.
- Save raw promotion text, eligibility, and context; distinguish missing values from zero.
- Reopen appended files; verify unchanged old-row hashes and valid new references. Excel and JSON must agree on the same commit.
- Read back scheduling timezone, frequency, and next run; check subsequent actual executions. Successful task creation is not successful capture.
- Resolve the real local directory and verify writes. When offline, mark only pending.

## Synthetic regressions (synthetic data or sandbox only; never fabricate data in a real cart)
1. Item A has quantity 2, displayed unit price CNY 100, and single-selection line total CNY 180: preserve quantity; distinguish derived unit price 90 from line total 180. Leave shipping blank without evidence.
2. Nothing selected, 3 cart rows, footer CNY 0: selected total is 0; whole-cart amount is unknown. Do not record an empty cart.
3. Cart has 20 rows, only 8 on the first screen, bottom not reached: PARTIAL, not COMPLETE.
4. Original selection A/C; selecting B triggers an error confirmation: cancel, restore A/C, verify. If restoration fails, stop UI actions and report.
5. Phone authentication in the cloud is complete but a stale login overlay remains: refresh the actual page to verify; on success cancel stale authentication. Do not repeatedly request verification codes.
6. A tool refuses before page reading: TOOL_POLICY_DENIED. Do not diagnose an overseas IP or switch proxies.
7. Synthetic C/D/F combination has total base price CNY 3200 and an explicit CNY 400 reduction at a CNY 3000 threshold, with all eligibility/order known: reconstruct CNY 2800; label RECONSTRUCTED, not OBSERVED_COMBO. Unknown coupon stacking does not permit an additional coupon reduction.
8. Only individually coupon-discounted C/D/F quotes exist, without undiscounted base prices/eligibility: unique reconstruction is impossible; explain missing inputs instead of adding quotes.
9. A time gap crosses a promotion's expiry: do not interpolate promotions. Without user request, leave PriceEstimates empty.
10. Excel succeeds but JSON fails for the same run: mark the manifest PARTIAL; finish the same commit next time without appending Excel twice or changing old facts.
11. Two concurrent runs: only the lock holder may operate the UI; the other records a conflict/skip.
12. Local Excel was user-edited: preserve it and save a separate conflict copy; never force cloud overwrite.
13. New combination chart fails while the single-item chart succeeds: replace only the successful chart counterpart. Keep the old combination chart and leave user photos untouched.
14. User computer is off: cloud capture continues if persistent capabilities exist; local work is pending. Do not “create” a fake cloud desktop.
15. Three days have elapsed without new data: still send the one agreed prompt for that period and explain data status. Do not draw or duplicate the reminder within the same period.

## Review evidence
For each deployment, retain a nonsensitive acceptance record: tool capabilities, test time, passed/failed/not-run status, evidence references, and remaining limits. Before a new version, perform structural checks and controlled scenario validation. Do not contaminate independent review with expected-answer hints. Never describe document review as a successful live-site end-to-end test.

## New-configuration and identity regressions
16. A single variant has explicit official fixed-configuration evidence but no SKU: analyze using the internal identity version and keep sku_id null. No selector but no single-variant evidence: unresolved; still capture, but do not merge history.
17. A SKU appears later: append mappings with time/evidence ranges; do not backfill old facts. A pack changing from 1 unit to 2 gets a new identity version. Two cart rows of the same identity receive separate quotes.
18. User chooses every 5 hours: use an elapsed-time anchor, not a daily-midnight reset. Daily without a time: propose 10:00 in the initial bundled confirmation. Do not claim activation when the tool lacks support.
19. Only UTC+08 is exposed: do not guess an IANA timezone or adopt cloud UTC as the user's timezone. Do not infer travel while the user is offline. Update and verify future schedules according to policy only upon a new trusted timezone signal; keep site region/currency unchanged.
20. Daylight-saving skipped/repeated times and timezone changes: wall-clock scheduling follows the disclosed policy; UTC instances remain unique; historical offsets stay unchanged. Coverage uses the then-effective version; do not resend a three-day reminder for the same period.
21. An item disappears from a PARTIAL page: do not claim deletion or zero price. Record an explicitly empty cart only after complete, logged-in verification. On account switching or handover to another dot, first verify permissions and dataset; do not join old-account history or create duplicate tasks.
22. One run takes longer than the capture interval: record and report skipped runs; do not operate parallel browsers or reduce frequency without authorization. Keep only one pending authentication request for a continuing login blocker, while retaining failure logs.

## Automation-first and delivery regressions
23. Current-user approval covers normal JD cart navigation: verify m.jd.com → mtrade.jd.hk cart scope without repeated domain prompts; an actual tool/security refusal still stops that route.
24. Ordinary graphical challenge with applicable grant: attempt supported UI; no grant where required: request approval. Repeated failure/device requirement: minimal handoff. Sixty seconds of silence never grants permission. New explicit legal agreement follows the current confirmation policy.
25. JD shopping and instant-delivery sections have varying counts: cover the requested scope and mark excluded/unread sections; never assume a previous count or footer zero proves an empty whole cart.
26. Deselection reverts during DOM rebuild: stop mutations, preserve partial quotes, distinguish actual navigation, and verify restoration. A phone clear plus refreshed cloud empty set does not prove the bug repaired. Original nonempty set is the default target; explicit leave-empty authorization is a recorded override.
27. Same verified product/quantity observed on desktop then mobile: one labeled timeline retains provenance and conditions. Conditional versus applied quotes are not silently compared as a true price drop.
28. Automatic transfer fails on unsupported OS metadata: preserve official tools, prepare verified ZIP fallback. No download toast: inspect permitted local files and hashes. Reject traversal, symlinks, duplicate paths, oversized archives, and mismatched hashes; preserve user-edited Excel/unknown files.
29. Chart send accepted but delivery or local extraction pending: retain image. Verified delivery with all consumers complete and recoverable-cleanup grant: clean only owned chart artifacts, never Excel/source/backup history. No safe Trash: report pending rather than purge.
