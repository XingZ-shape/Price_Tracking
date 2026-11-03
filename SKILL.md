---
name: price-tracking
description: Continuously capture complete cart prices, individually selected and specified-combination quotes, and promotion history from JD, Taobao, or other shopping sites. Store Excel history in the cloud; reconstruct combination prices, chart trends, and sync to the user's desktop on request. Use to configure or maintain tracking with customizable frequency and automatic user-timezone following, check capture status, add combinations, estimate unobserved combination history, redraw charts, renew, or handle “数据同步到我的本地” (sync data to my local computer).
---
# Shopping Cart Price History Tracking

## Objective and boundaries
Build a traceable history, not a shopping agent. Support separate curves for individually selected item quotes, observed quotes for predefined combinations, and historical conditional estimates for combinations requested later. Minimize user effort without presenting unavailable capabilities, unauthorized actions, or unknown data as completed automation.

This skill supplies a reproducible workflow, data contract, and acceptance criteria; it does not guarantee identical capabilities or prices across sites, accounts, or tool environments. Read current tool instructions and the latest user restrictions. Installing this skill does not inherit another user's permissions, sessions, files, or scheduled tasks.

## Routing and required reading
- First use or account/site migration: read [onboarding-auth.md](references/onboarding-auth.md).
- Every capture, including an immediate manual capture: read [capture.md](references/capture.md), [storage-schema.md](references/storage-schema.md), and [automation-recovery.md](references/automation-recovery.md).
- Historical combination estimation or trend interpretation: read [analysis.md](references/analysis.md) and consult the field dictionary.
- Charts, renew, or local synchronization: read [charts-sync.md](references/charts-sync.md).
- After establishing or changing the workflow: perform applicable checks in [acceptance.md](references/acceptance.md).
- Configuration initialization: copy [config.template.json](assets/config.template.json), populate confirmed settings only, and leave unconfirmed settings null or false. Track locally managed files with [local-manifest.template.json](assets/local-manifest.template.json).
- Scheduled-task creation: instantiate [scheduled-run.template.md](assets/scheduled-run.template.md); the template is not a created task.

## One-time introduction
Use the user's language for messages, explanations, and chart labels; English skill instructions do not change the user's language preference. Begin briefly rather than presenting the whole technical document:
> I will regularly refresh your cart and save item prices, selection-based quotes, and promotion history. I will not buy anything, change quantities, claim coupons, or pay. I will first complete permitted login and verification steps myself; I will involve you only for steps that require your device, credentials, approval, or sustained verification failure. Observations and estimates are stored separately; missing data is never fabricated. Continuous cloud operation requires persistent tools. I generate charts only when you ask and update desktop data only when you request a local sync.
> Key commands: start tracking / capture now / report progress / record combination “name: items…” / estimate a combination's history / draw charts / sync data to my local computer (or renew) / pause / resume / cancel / change frequency.
Retain equivalent Chinese command aliases: 开始追踪 / 现在采集 / 报告进度 / 记录组合「名称：商品…」/ 估算组合…的历史 / 画图 / 数据同步到我的本地 (or renew) / 暂停 / 恢复 / 取消 / 修改频率; also recognize 同步到本地 as local sync.
Then confirm only the missing site, timezone, cloud-privacy consent, and action permissions. Offer defaults from the references in one bundled question. Do not re-ask questions already answered explicitly.

## End-to-end workflow
1. **Check capabilities:** Discover the current cloud browser, persistent storage, workbook editing, real scheduler, notification channel, and optional local connection. Read their tool/skill instructions first. Explain any missing capability and resulting limitation; do not simulate unavailable functionality.
2. **Configure:** Confirm site/account isolation, timezone, frequency, cloud storage, and selection scope. Support every N hours, daily, or chosen local wall-clock times. Initially show the default of every 4 hours and automatic following of the verified user timezone; do not repeat already-granted authorization. See the scheduling reference. Combinations can be added later and must not block basic capture.
3. **Verify login:** Perform supported and authorized authentication steps on the official page in the normal cloud browser; request secure user input only when needed. Verify the actual cart; “the user says they logged in” is not page evidence.
4. **Prepare destinations:** Verify or establish the sole authoritative cloud workbook and source-data identities. On first use, create authorized Price tracking/data and Price tracking/image folders on the user's desktop. If local access is unavailable, record creation as pending without blocking authorized cloud work. Never call a cloud path the user's desktop.
5. **Perform initial technical validation:** Refresh, capture the complete cart and authorized quotes, save real results, and show brief samples, field structure, missing fields, and restoration status. Enable recurring tasks only after first-run acceptance; do not request approval again if already explicitly given.
6. **Create and verify scheduling:** Check existing tasks for the dataset to avoid duplicates. Save through a real scheduler and read back timezone, schedule, status, notification conversation, and next run if exposed. If next_run is unavailable, report only an estimated planned time. Do not claim continuous operation without save and readback evidence.
7. **Capture each run:** Lock, load latest versions, refresh, cover the entire cart, obtain individual quotes and specified-combination quotes, preserve promotion evidence, restore selections, append and commit, verify by reopening, and report briefly. Save failure facts too; never invent prices.
8. **Maintain:** Request only necessary user login when the session expires. Safely repair semantic UI targeting and validate after page changes. Immediately report selections that cannot be restored. Keep full snapshots even when prices are unchanged.
9. **Analyze on request:** Generate useful default charts only when requested. Optionally ask about previously unrecorded combinations the user wants to revisit, without requiring an answer. Without a request, only send an approved periodic chart offer; do not draw automatically.
10. **Sync locally on request:** Trigger from the user's latest request; persistent cloud data remains authoritative. Validate new files before atomically updating this skill's local latest files, protecting user edits. Clean up only skill-owned old charts after successful completion of replacement charts; never delete unrelated files.

## Invariants
- Refresh the normal cart page every run and record refresh and observation times. Do not promise cached prices are current. Stop claiming successful current-price capture if refresh fails.
- Do not bypass CAPTCHAs, risk controls, or browser security policies; do not evade refusal using proxies/VPNs or extract credentials/session secrets.
- Record the original selection before toggling. Change selections only within authorization, preserve quantities, and re-read UI targets each time rather than reusing stale indices. On completion or failure, attempt and verify restoration to the baseline or latest explicitly authorized final selection; stop further toggling if restoration fails.
- Do not add/remove items, change quantities, claim coupons, enter checkout, order, or pay. Keep the checkout table empty by default; separate explicit authorization still requires checking current safety rules.
- Separate whole-cart, current-selection, single-item, and combination scopes. Never call a selection quote an amount actually paid; an unselected total of 0 does not mean the entire cart costs 0.
- Show one product timeline across desktop/mobile sources when identity and quantity match, retaining source/context provenance. Do not treat different regions, eligibility, or price semantics as directly comparable, or join different identity versions/quantities without explicit normalization. Do not guess unknown IDs.
- Visible promotion ≠ eligibility ≠ claimed coupon ≠ stackability. Use Unknown for unknown conditions, leave missing amounts blank, and use 0 only when observed.
- Append complete history to the master. Deduplicate retries, not genuine new scheduled observations. Commit source data and Excel together, re-read and merge on version conflicts, and preserve history and raw rule text.
- Store raw observations, historical rule reconstructions, and temporal imputations separately. Never label modeled results as observed or fabricate missed periods.
- Do not collect unnecessary account/address details or store passwords, SMS codes, cookies, tokens, login QR codes, or full-page screenshots.
- Claim completion only with verification evidence. Unsaved captures are not successfully stored; a successful current run does not establish future scheduling reliability.

## Automation-first execution
Act on existing, specific permissions without repeatedly requesting the same approval. Use normal supported flows first, including the JD mobile-web workflow in capture.md and the automatic-transfer-first ZIP fallback in charts-sync.md. Explain possible manual steps once at onboarding. A portable skill is not itself a grant from its next user: record the current user's scope and obey current tool and confirmation rules. Silence, elapsed time (including one minute), and defaults never supply missing consent. Keep explicit legal-agreement and mandatory action-time approvals; do independent authorized work while approval is pending. Preserve Excel/source history indefinitely unless the user separately requests a permissible retention change; chart cleanup never authorizes deleting data.
