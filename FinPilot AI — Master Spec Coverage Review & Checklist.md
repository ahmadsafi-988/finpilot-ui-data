# FinPilot AI — Master Spec Coverage Review & Checklist

Nov 7, 2026 · @Ahmad safi

## Review outcome

Round 1 of the HTML prototype covers onboarding and the Summary end to end; of the 20 modules, 3 are fully covered, 13 partly and 4 not yet designed, so design sign-off remains open.

| Item | Value |
| --- | --- |
| Source of truth | FP-MASTER-FR-001 v1.0, Master Functional Requirements, User Flows & Role Access (7 Nov 2026) |
| Design evidence | `finpilot-prototype.html` — clickable HTML prototype, round 1 |
| Scope inspected | 51 business rules, 20 modules, 34 flows, 46 user stories, 33 KPIs, 22 chart subjects, 11 UI states, 4 roles |
| Not in scope | Backend behaviour, runtime security, real calculations, usability testing with real users |

The prototype implements FL-01 to FL-10 and FL-19 in English and Arabic, plus the CEO approval drawer from FL-26. Every other module is reachable from the role-aware sidebar but shows a placeholder with its route and spec IDs. This is an expert inspection of design evidence, not Product Owner approval.

### Reading the coverage statuses

| Status | Meaning |
| --- | --- |
| Covered | The interaction or distinction is designed and clickable in the prototype. It does not prove the application works. |
| Partial | Some screens, states or examples exist; variants or detail remain. |
| Specified | A backend or behavioural rule is documented in the spec; a UI cannot show it. Needs implementation tests. |
| Not designed | The spec is complete enough to design, but only a route stub exists. |
| Decision | Final behaviour depends on an open item in §10.4 or another unresolved question. |

## Business-rule checklist

24 of 51 rules are covered in the prototype; the rest are partial (14), backend-only (4), not yet designed (7) or waiting on a decision (2). Rules are derived from the spec sections cited; BR-50 records the metrics-snapshot choice made in conversation, not yet written into the spec.

| Rule | Requirement | Spec source | Prototype evidence | Status | Evidence or remaining work |
| --- | --- | --- | --- | --- | --- |
| BR-01 | One company workspace per account; all data tenant-isolated | §1.4, FR-IAM-004, FR-IAM-010 | Create Workspace copy | Specified | Tenant isolation and second-membership rejection need backend tests. |
| BR-02 | Four role templates plus per-action grant/revoke overrides | §1.6, §5.1, FR-IAM-012–015 | View-as switcher; sidebar per §4.6 | Partial | Role & Permission Editor (FL-28) not designed. |
| BR-03 | Effective access enforced server-side; hidden menu is only a convenience | FR-IAM-016–017, FR-AUTH-011 | Access-denied page text | Specified | 403 on direct API calls needs implementation tests. |
| BR-04 | CEO has permanent full access; owner can never be removed | FR-IAM-001, FR-IAM-005, FR-IAM-025 | CEO sees every module | Partial | Show owner protection (locked toggles) in the permission editor. |
| BR-05 | Admin may grant/revoke own permissions without CEO approval; audited | FR-IAM-019–021, FL-29 | Denied page links to My Permissions | Not designed | My Permissions screen and self-grant preview not designed. |
| BR-06 | Accountant: finance CRUD, imports, reports, Lab, AI; no admin by default | §4.3 | Accountant sidebar and Summary | Covered | — |
| BR-07 | Consultant starts with profile only; finance only by explicit grant | §4.5 | Consultant lands on Profile | Partial | Granted read-only variant (US-041) not designed. |
| BR-08 | Owner sign-up: owner role server-assigned; no account enumeration | FL-01, FR-AUTH-002–006 | Sign-up, Verify Email (sent / expired / resend) | Covered | — |
| BR-09 | Invites are company-bound, expiring, single-use; one company per user | FL-11, FL-12, FR-AUTH-013–015 | — | Not designed | Invite form, pending list, Accept Invitation and expired-invite screens. |
| BR-10 | Workspace and owner membership created atomically and idempotently | FL-03, FR-ONB-002–004 | Create Workspace note | Specified | Retry behaviour needs backend test. |
| BR-11 | One company currency; restricted change after records exist | FR-CFG-006, §5.4 | Financial Setup hint | Partial | Settings-screen block on unsafe change not designed. |
| BR-12 | Unknown opening cash is never treated as JOD 0 | FR-ONB-006, §6.4 | "I don't know this yet" → Unknown; D1 Needs Attention | Covered | — |
| BR-13 | Onboarding by CSV or manual entry, mixable later | FR-ONB-008, FR-ONB-016 | Data-method choice; in-app CSV Import | Covered | — |
| BR-14 | No separate Finish Setup screen; Readiness goes straight to Summary | FR-ONB-015, §1.5 | Readiness → Go to Summary | Covered | — |
| BR-15 | Confirmed None is distinct from Not Provided | FR-ONB-012 | Readiness and manual hub actions | Covered | — |
| BR-16 | CSV covers D2–D4 only; obligations and adjustments are manual | §1.4, FR-IMP-001–003 | Unsupported obligations.csv message | Covered | — |
| BR-17 | AI mapping shows confidence; ambiguous money/sign never guessed; manual fallback | FL-06, FR-IMP-005–007 | Mapping table; blocking Dr/Cr decision; AI-off mode | Covered | — |
| BR-18 | Duplicates flagged, never auto-discarded; explicit decision required | FR-IMP-011–012 | Exact (trx row 5) and potential (INV-2041) examples | Covered | — |
| BR-19 | Any edit invalidates earlier validation | FR-IMP-013 | Banner switches to "validate again" | Covered | — |
| BR-20 | Batch commits all-or-nothing and idempotently | FR-IMP-016–017 | Simulated DB failure → 0 records, draft kept | Covered | Idempotent retry needs backend test. |
| BR-21 | No reusable mapping templates in MVP | §1.4 | Not offered | Specified | — |
| BR-22 | Only D2 transactions move actual cash; paid-to-date never posts cash | §6.2, FR-AR-013, FR-AP-013 | Form notes; KPI drawer; import cautions | Covered | — |
| BR-23 | Transaction: positive amount, IN/OUT, cash-flow class required | FR-TRX-007–008 | Transaction modal validation | Covered | — |
| BR-24 | Paid-to-date between 0 and gross | FR-AR-008, FR-AP-007 | Review-table and modal errors | Covered | — |
| BR-25 | Soft delete only; history and audit kept | FR-TRX-005, FR-AR-005, FR-AP-005, FR-OBL-006, FR-ADJ-005 | — | Not designed | Archive confirmation and record-history drawer for all five record types. |
| BR-26 | Supplier bill / obligation overlap flagged for human resolution | FR-AP-011, FR-OBL-013, §6.6 | Note in supplier-invoice modal | Decision | Overlap-resolution UI; behaviour not defined in spec (see D-19). |
| BR-27 | Recurring obligations create dated events, never cash postings | FR-OBL-011 | Obligation modal note | Partial | Occurrence calendar and schedule preview. |
| BR-28 | Adjustments are non-cash; only confirmed count; no double recognition; missing facts requested | FR-ADJ-009–014 | Adjustment modal note | Partial | Suggestion queue, draft/confirmed states, useful-life request. |
| BR-29 | AI classifications need human confirmation; never auto-posted | FR-AI-009–010, FR-TRX-011 | "Use suggestion" chip with 86% confidence | Covered | — |
| BR-30 | 33 core KPIs; #18 and #21 deferred | §6.5, FR-KPI-001 | Readiness list of all 33 | Partial | KPI Directory (FL-31) not designed. |
| BR-31 | No invented zeros; zero denominator shows N/A | §6.4, FR-KPI-013 | Insufficient Data cards; Runway N/A; states gallery | Covered | — |
| BR-32 | Period values and as-of values clearly labelled | FR-SUM-004–005 | Period, As-of and Forecast pills | Covered | — |
| BR-33 | Upcoming Payments (#23) ≠ Expected Outflows (#28) | §6.6 | Both shown with explanatory note | Covered | — |
| BR-34 | Summary: 7 sections, 6 KPIs, 3 chart slots chosen by subject, saved per user | §7.2, FR-SUM-001–020 | Summary page; Change Chart library; Reset | Covered | — |
| BR-35 | 22 chart subjects; missing data shows reason, never demo values in production | §7.3 | View All Charts; Insufficient states | Covered | Jun–Sep history in prototype is labelled demo data. |
| BR-36 | Historical AR/AP aging not reconstructed without dated payments | §6.8 | Aging chart note | Covered | — |
| BR-37 | Reports generated in-app, saved as snapshots; regenerate creates new version; no PDF | FR-REP-001–020, §1.4 | — | Not designed | Report Generator, Viewer (11 sections), Saved Reports. |
| BR-38 | Founding date shown separately from first record date | FR-REP-018 | — | Not designed | All-available-history report variant. |
| BR-39 | Standalone 30/90-day forecast with labelled assumptions and source events | FR-FCT-001–019 | Summary section 4 only | Partial | Forecast page, event table, assumptions drawer. |
| BR-40 | Decision Lab uses hypothetical line items only; isolated; private by default | FR-LAB-001–021, §6.7 | HYPOTHETICAL badge; scenario chart empty state | Partial | Scenario list, item editor, results, compare, share. |
| BR-41 | AI answers use authorized tools, show evidence, take no external action | FR-AI-001–016 | "How calculated" drawer | Partial | Chat, history, refusal and tool-failure states. |
| BR-42 | AI-proposed tasks need confirmation; task links grant no data access | FR-TSK-012, FR-TSK-014 | Task list on Summary | Partial | Task create/detail and AI task proposal. |
| BR-43 | Policy-gated change: pending → approved/rejected → committed once after revalidation | §5.3, FR-APR-001–014 | Review drawer; pending submit from forms and import | Covered | — |
| BR-44 | Admin configures approval policies but cannot approve | FR-APR-015, US-039 | No approve buttons for non-CEO | Partial | Approval Policy Editor not designed. |
| BR-45 | CEO-initiated action when policy requires CEO approval | §5.3, FR-APR-020 | Labelled placeholder in forms | Decision | Open decision D-01. |
| BR-46 | Alerts deterministic, deduplicated, no anomaly detection; email for important | FR-ALT-001–014 | Summary alerts, alert and notification drawers | Partial | Alerts page and threshold settings. |
| BR-47 | Audit is append-only; self-grants and policy edits logged | FR-AUD-001–010 | — | Not designed | Audit list, filters, before/after view. |
| BR-48 | Permissions rechecked at commit; stale forms rejected | FL-34, FR-IAM-022–023 | — | Not designed | Show 403 refresh state inside an open form. |
| BR-49 | Full Arabic RTL and English LTR | §7.1, §7.7 | Onboarding and Summary in both | Partial | Remaining modules once designed. |
| BR-50 | Metrics snapshot recalculates after every committed change and daily | Conversation decision (not yet in spec) | Snapshot bar; "Updating metrics…" | Covered | Add to §10.4 as a confirmed implementation choice. |
| BR-51 | MVP exclusions respected: no bank feeds, payments, reconciliation, multi-currency, PDF | §1.4 | Nothing excluded is offered | Covered | — |

## Module coverage

Onboarding, Summary and CSV Import are complete; Decision Lab and the three administration modules (IAM, CFG, AUD) have no screens yet beyond a route stub.

| Module | Area | Requirements | Flows | Prototype evidence | Status | Limitation / next design work |
| --- | --- | --- | --- | --- | --- | --- |
| AUTH | Authentication & session | FR-AUTH-001–015 | FL-01, FL-02, FL-12 | Landing, sign-up, verify email, log in, forgot password | Partial | Set-new-password, invitation acceptance, profile & security, suspended-account state. |
| ONB | Workspace & financial onboarding | FR-ONB-001–016 | FL-03–FL-05, FL-09, FL-10 | Create workspace, financial setup, data method, manual hub, readiness | Covered | — |
| SUM | Summary / CEO homepage | FR-SUM-001–020 | FL-19, FL-33 | 7 sections, 6 KPI drawers, chart library, reset, period selector | Covered | — |
| TRX | Cash transactions | FR-TRX-001–015 | FL-13 | Create modal with AI category suggestion | Partial | List, filters, details drawer, edit, archive. |
| AR | Customer invoices | FR-AR-001–013 | FL-14 | Create modal (gross/outstanding preview); import review | Partial | List with status filters, details drawer, aging view. |
| AP | Supplier invoices | FR-AP-001–014 | FL-15 | Create modal; import review | Partial | List, due filters, drawer, obligation-overlap warning. |
| OBL | Financial obligations | FR-OBL-001–015 | FL-16 | Create modal (one-time/monthly/quarterly) | Partial | List, occurrence calendar, schedule preview, paid-to-date per occurrence. |
| ADJ | Accounting adjustments | FR-ADJ-001–016 | FL-17 | Create modal with non-cash note | Partial | List, generated suggestion queue, draft/pending/confirmed states, source link. |
| IMP | CSV import & mapping | FR-IMP-001–018 | FL-05–FL-08 | Upload, mapping, review, confirm, result (success, rollback, pending approval) | Covered | — |
| KPI | 33 KPI engine | FR-KPI-001–013 | FL-31 | Formula/evidence drawers for the 6 Summary KPIs | Partial | KPI Directory with all 33 and period/as-of selector. |
| REP | Reports & analytics | FR-REP-001–020 | FL-20, FL-32, FL-33 | View All Charts (22 subjects) | Partial | Report Generator, 11-section Viewer, Saved Reports, Regenerate and compare. |
| FCT | Official cash-flow forecasting | FR-FCT-001–019 | FL-21 | Summary section 4 (30/90 toggle), projected-balance charts | Partial | Standalone page, dated event table, assumptions drawer, source drill-down. |
| LAB | Financial Decision Lab | FR-LAB-001–021 | FL-22 | HYPOTHETICAL badge; empty "Baseline vs Scenario" chart | Not designed | Scenario list, line-item editor, results, compare, share. |
| AI | Ask FinPilot AI | FR-AI-001–016 | FL-18, FL-23 | Insight card, How-calculated drawer, AI suggestion chips | Partial | Chat, conversation history, evidence drawer, refusal and failure states. |
| ALT | Alerts & notifications | FR-ALT-001–014 | FL-24 | Summary alerts, alert drawer, notification drawer, acknowledge | Partial | Alerts page with filters; threshold settings. |
| TSK | Internal tasks | FR-TSK-001–014 | FL-25 | Task list on Summary | Partial | My/Team Tasks, New Task, task detail, AI proposal review. |
| APR | Approvals & policies | FR-APR-001–020 | FL-26, FL-27 | Review drawer, approve/reject, pending submits | Partial | Approval history, Approval Policy Editor, stale-request state. |
| IAM | Team, roles & permissions | FR-IAM-001–025 | FL-11, FL-28, FL-29, FL-34 | View-as switcher and role sidebars only | Not designed | Team list, invitations, role editor, My Permissions, active-session revoke. |
| CFG | Application configuration | FR-CFG-001–015 | FL-30 | — | Not designed | Company, financial, alert and application settings screens. |
| AUD | Audit logs | FR-AUD-001–010 | FL-30 | — | Not designed | Audit list, filters, before/after event detail. |

## Flows, user stories and QA coverage

The prototype can walk through 11 of 34 flows end to end; 13 are partly clickable and 10 have no screens yet. Each flow maps one-to-one to a user story and QA case (§10.1).

### Flow coverage (FL-01 – FL-34)

| Flow | Journey | Story / QA | Requirement IDs | Prototype evidence | Status | Review result / remaining work |
| --- | --- | --- | --- | --- | --- | --- |
| FL-01 | Owner sign-up and email verification | US-001 / QA-FL-01 | FR-AUTH-001–006; FR-ONB-001 | Sign-up with strength meter; verify sent, expired, resend | Covered | Consent placeholder pending D-07. |
| FL-02 | Login, logout, reset password, session | US-002 / QA-FL-02 | FR-AUTH-007–012 | Log in, generic error, forgot password, log out, resume | Partial | Set-new-password screen; session-expired only in gallery. |
| FL-03 | Create company workspace | US-003 / QA-FL-03 | FR-ONB-002–004; FR-IAM-001 | Form with inline validation | Covered | — |
| FL-04 | Initial financial setup | US-004 / QA-FL-04 | FR-ONB-005–008; FR-CFG-004 | Currency, fiscal year, opening cash or Unknown | Covered | — |
| FL-05 | Choose CSV onboarding | US-005 / QA-FL-05 | FR-IMP-001–003 | Upload with declared type; unsupported-file error | Covered | — |
| FL-06 | AI column mapping and classification | US-006 / QA-FL-06 | FR-IMP-004–007; FR-AI-008 | Header vs field, confidence bars, Dr/Cr decision, AI-off mode | Covered | — |
| FL-07 | Editable review and duplicates | US-007 / QA-FL-07 | FR-IMP-008–013 | Tabbed editable table, row errors, exact/potential duplicates, Validate | Covered | Bulk edit and pagination for large files not shown. |
| FL-08 | Atomic CSV confirmation and commit | US-008 / QA-FL-08 | FR-IMP-014–018 | Totals, exclusions, cautions, commit, rollback, pending approval | Covered | — |
| FL-09 | Manual onboarding records | US-009 / QA-FL-09 | FR-ONB-009–012 | Hub with Add record and We have none for D2–D5 and ADJ | Covered | — |
| FL-10 | Readiness and Summary entry | US-010 / QA-FL-10 | FR-ONB-013–016; FR-SUM-001 | Five statuses, KPI coverage x/33, Go to Summary | Covered | — |
| FL-11 | Invite Company Admin and team | US-011 / QA-FL-11 | FR-IAM-006–010 | — | Not designed | Invite form, pending list, cancel/resend. |
| FL-12 | Invited user activation | US-012 / QA-FL-12 | FR-AUTH-013–015; FR-IAM-011 | — | Not designed | Accept-invite, expired, already-in-another-company states. |
| FL-13 | Transaction CRUD | US-013 / QA-FL-13 | FR-TRX-001–015 | Create modal; cash recalculated after save | Partial | List, drawer, edit, archive. |
| FL-14 | Customer invoice lifecycle | US-014 / QA-FL-14 | FR-AR-001–013 | Create modal with gross/outstanding preview | Partial | List filters, drawer, paid-to-date update, archive. |
| FL-15 | Supplier invoice lifecycle | US-015 / QA-FL-15 | FR-AP-001–014 | Create modal with cost classification | Partial | List, overdue view, overlap warning. |
| FL-16 | Financial obligation lifecycle | US-016 / QA-FL-16 | FR-OBL-001–015 | Create modal with recurrence | Partial | Occurrences, per-occurrence paid-to-date, archive. |
| FL-17 | Accounting adjustment CRUD and generation | US-017 / QA-FL-17 | FR-ADJ-001–016 | Manual create modal | Partial | Source-linked suggestions and confirmation flow. |
| FL-18 | AI classification review | US-018 / QA-FL-18 | FR-AI-007–011 | AI chip in transaction form; mapping confidence | Partial | "Needs Review" state for unsafe suggestions. |
| FL-19 | View Summary and customize subjects | US-019 / QA-FL-19 | FR-SUM-001–020 | Full Summary, Change Chart, Reset, View All | Covered | — |
| FL-20 | Generate historical report | US-020 / QA-FL-20 | FR-REP-001–020 | — | Not designed | Generator, coverage preview, 11-section viewer. |
| FL-21 | Inspect 30/90-day forecast | US-021 / QA-FL-21 | FR-FCT-001–019 | Summary forecast section and chart | Partial | Standalone page, event table, assumption edit. |
| FL-22 | Decision Lab scenario lifecycle | US-022 / QA-FL-22 | FR-LAB-001–021 | — | Not designed | Whole module. |
| FL-23 | Ask FinPilot AI with evidence | US-023 / QA-FL-23 | FR-AI-001–016 | Insight card, How calculated | Partial | Chat, history, refusals, task proposal. |
| FL-24 | Receive and manage alerts | US-024 / QA-FL-24 | FR-ALT-001–014 | Alerts list, alert drawer, acknowledge | Partial | Alerts page filters, email delivery status. |
| FL-25 | Create, assign and complete task | US-025 / QA-FL-25 | FR-TSK-001–014 | Read-only task list | Partial | Create, assign, status change, history. |
| FL-26 | Submit, approve, reject controlled change | US-026 / QA-FL-26 | FR-APR-001–014 | Pending submit, review drawer with before/after, approve, reject | Partial | Stale-source and approved-but-not-applied states. |
| FL-27 | Configure approval policies | US-027 / QA-FL-27 | FR-APR-015–020 | Demo toggle only | Not designed | Policy Editor with version and effect warning. |
| FL-28 | Manage team, roles, permissions | US-028 / QA-FL-28 | FR-IAM-012–023 | — | Not designed | Role & Permission Editor with sidebar preview. |
| FL-29 | Admin self-grant / revoke | US-029 / QA-FL-29 | FR-IAM-019–025 | Denied page pointer only | Not designed | My Permissions with preview and audit note. |
| FL-30 | Configure app, inspect audit | US-030 / QA-FL-30 | FR-CFG-001–015; FR-AUD-001–010 | — | Not designed | Settings and audit screens. |
| FL-31 | Explore KPI directory and evidence | US-031 / QA-FL-31 | FR-KPI-001–013 | 6 KPI drawers | Partial | Directory of all 33 KPIs. |
| FL-32 | Review saved reports and rerun | US-032 / QA-FL-32 | FR-REP-015–020 | — | Not designed | Snapshot list, regenerate, compare versions. |
| FL-33 | View all chart subjects | US-033 / QA-FL-33 | FR-SUM-012–020; FR-REP-010 | All 22 subjects by category, Set on Summary | Covered | — |
| FL-34 | Active-session permission change | US-034 / QA-FL-34 | FR-IAM-022–025 | — | Not designed | 403 inside an open form, then refreshed menu. |

### Additional role stories (US-035 – US-046)

| Story | Role | Requirement trace | Negative test | Status | Note |
| --- | --- | --- | --- | --- | --- |
| US-035 | CEO | FR-IAM-001–005; FR-IAM-025 | No editor removes owner rights | Partial | All modules visible; owner lock not yet shown. |
| US-036 | Accountant | FR-TRX, FR-AR, FR-AP, FR-OBL, FR-ADJ | Cannot edit team permissions | Partial | Create modals exist; list pages do not. |
| US-037 | Accountant | FR-REP-001–020; FR-FCT-001–019 | Assumption write rejected without grant | Not designed | Needs forecast page (and D-02). |
| US-038 | Company Admin | FR-IAM-006–015 | Invite from another company cannot be accepted | Not designed | — |
| US-039 | Company Admin | FR-APR-014–020 | Admin approve attempt fails | Partial | Admin sees no approve action; policy editor missing. |
| US-040 | Company Admin | FR-IAM-018–025 | Owner access immutable; self-grant audited | Not designed | — |
| US-041 | Consultant | FR-IAM-013–017; FR-REP-015–018 | Transaction API returns 403 without read grant | Not designed | Needs a granted read-only variant. |
| US-042 | Consultant | FR-TSK-002–008 | Linked finance source hidden | Not designed | — |
| US-043 | CEO | FR-FCT-001–019 | Missing dates shown as assumptions | Partial | Summary section only. |
| US-044 | CEO | FR-LAB-001–021 | Official cash unchanged by simulation | Not designed | — |
| US-045 | CEO | FR-AI-001–016 | Missing source returns uncertainty | Partial | How-calculated drawer only. |
| US-046 | Company Admin | FR-AUD-001–010; FR-IAM-019–024 | Audit cannot be soft-deleted | Not designed | — |

## Role, KPI, chart and state coverage

All four role sidebars match §4.6, 28 of 33 KPIs are fully shown and 3 more partly, all 22 chart subjects render, and all 11 universal states have an example.

### Role access (§4)

| Role | Default landing | Sidebar vs §4.6 | Summary | Quick create | Approve | Gaps |
| --- | --- | --- | --- | --- | --- | --- |
| CEO | Summary | Matches | Full, 7 sections | All five record types + CSV | Yes, review drawer | Owner-protection lock in permission editor |
| Accountant | Summary | Matches (My Tasks & Requests) | Full; tasks show "My requests" | All five + CSV | No; sees request status | Read-only forecast assumptions (US-037) |
| Company Admin | Team & Access | Matches; finance hidden | Access denied → My Permissions | Hidden | No | Self-grant flow and resulting sidebar |
| Consultant | Profile | Matches; profile only | Access denied | Hidden | No | Granted read-only variant |

### KPI coverage (§6.5)

| # | KPI | Scope | Sources | Shown in prototype | Status |
| --- | --- | --- | --- | --- | --- |
| 1 | Current Cash Balance | As-of | D1, D2 | Summary card, drawer with opening + IN − OUT | Covered |
| 2 | Cash Received | Period | D2 | Net Cash Flow drawer; Received vs Paid chart | Covered |
| 3 | Cash Paid | Period | D2 | Net Cash Flow drawer; Received vs Paid chart | Covered |
| 4 | Net Cash Flow | Period | D2 | Summary card, drawer, trend chart | Covered |
| 5 | Operating Cash Flow | Period | D2 | Operating Cash Flow Trend chart | Covered |
| 6 | Cash Runway | As-of estimate | D1, D2 | Summary card, drawer, N/A rule | Covered |
| 7 | Revenue | Period | D3 | Summary card, invoice evidence drawer | Covered |
| 8 | Revenue Growth (%) | Comparable period | D3 | Card delta; growth trend chart | Covered |
| 9 | Total Expenses | Period | D4, D5, ADJ | Summary card, category drawer | Covered |
| 10 | Expenses by Category | Period | D4, D5, ADJ | Drawer and chart | Covered |
| 11 | Gross Profit | Period | D3, D4/ADJ | Net Profit bridge; profit comparison chart | Covered |
| 12 | Gross Profit Margin (%) | Period ratio | Derived | Margins chart | Covered |
| 13 | Operating Profit (EBIT) | Period | D3, D4, D5, ADJ | Net Profit bridge | Covered |
| 14 | Operating Profit Margin (%) | Period ratio | Derived | Margins chart | Covered |
| 15 | Net Profit / Loss | Period | D3, D4, D5, ADJ | Summary card, bridge drawer | Covered |
| 16 | Net Profit Margin (%) | Period ratio | Derived | Net Profit drawer | Covered |
| 17 | EBITDA | Period | D3, D4, D5, ADJ | Net Profit drawer | Covered |
| 19 | Unpaid Customer Invoices | As-of | D3 | Receivables & Payables | Covered |
| 20 | Overdue Customer Invoices | As-of | D3 | Receivables & Payables; alert | Covered |
| 22 | Unpaid Supplier Bills | As-of | D4 | Receivables & Payables | Covered |
| 23 | Upcoming Payments | Next 30 days | D4, D5 | Receivables & Payables; breakdown chart | Covered |
| 24 | Overdue Supplier Bills | As-of | D4 | Receivables & Payables; alert | Covered |
| 25 | 30-Day Cash Forecast | Future 30d | D1–D5 | Forecast & Liquidity (30 days) | Covered |
| 26 | 90-Day Cash Forecast | Future 90d | D1–D5 | Forecast & Liquidity (90 days) | Covered |
| 27 | Expected Inflows | Horizon | D3 | Forecast & Liquidity; IN vs OUT chart | Covered |
| 28 | Expected Outflows | Horizon | D4, D5 | Forecast & Liquidity; IN vs OUT chart | Covered |
| 29 | Projected Cash Shortfall | Horizon | Forecast events | First-shortfall strip; timeline chart; alert | Covered |
| 30 | Scenario Financial Impact | Scenario horizon | LAB + baseline | Empty Baseline vs Scenario chart only | Not designed |
| 31 | Sales Tax Rate | Invoice | D3, D4 | Tax % field in invoice form and import | Partial |
| 32 | Sales Tax Amount | Invoice | D3, D4 | Gross preview in invoice form | Partial |
| 33 | Tax Payable | As-of | D5 | Note under Tax Payments chart | Partial |
| 34 | Tax Paid | Period | D2 | Tax Payments & Liabilities chart | Covered |
| 35 | Upcoming Tax Payments | Future 30/90d | D5 | — | Not designed |

\#18 EBITDA Margin and #21 Days Sales Outstanding are deferred by the spec and correctly absent.

### Chart subjects (§7.3)

| Category | Subject | Minimum data | Prototype rendering |
| --- | --- | --- | --- |
| Business Performance | Revenue vs Expenses | Revenue/expense history | Grouped bars, Jun–Oct 2026 (default slot 1) |
| Business Performance | Revenue Growth Trend | Comparable revenue history | Line, Jul–Oct 2026 |
| Business Performance | Net Profit Trend | Recognized profit inputs | Line, Jun–Oct 2026 |
| Business Performance | Gross / Operating / Net Profit | P&L bridge | Bars, Oct 2026 |
| Business Performance | Profit Margins Comparison | Non-zero revenue | Bars (%), Oct 2026 |
| Business Performance | Expenses by Category | Confirmed classification | Horizontal bars, Oct 2026 |
| Business Performance | Revenue by Category | Revenue category on invoices | Horizontal bars, Oct 2026 |
| Cash & Liquidity | Net Cash Flow Trend | Dated D2 | Signed bars (default slot 2) |
| Cash & Liquidity | Cash Received vs Cash Paid | D2 IN/OUT by period | Grouped bars |
| Cash & Liquidity | Historical Cash Balance | Opening cash + D2 | Line, month-end |
| Cash & Liquidity | Operating Cash Flow Trend | D2 cash-flow class | Signed bars |
| Cash & Liquidity | Cash Runway Trend | Trailing burn per point | Line with Sep 2026 shown as N/A |
| Invoices & Obligations | Receivables vs Payables | Current AR/AP | Two bars, as of 6 Nov |
| Invoices & Obligations | Customer Receivables Aging | Due dates + paid-to-date | Buckets, current snapshot only |
| Invoices & Obligations | Supplier Payables Aging | Due dates + paid-to-date | Buckets, current snapshot only |
| Invoices & Obligations | Upcoming Payments Breakdown | Non-overdue AP + obligations | Horizontal bars, excludes SB-1038 |
| Invoices & Obligations | Outstanding Invoices by Customer | Customer IDs + AR | Horizontal bars |
| Invoices & Obligations | Tax Payments & Liabilities | D2 tax + D5 liability | Bars + payable note |
| Forecasting | Projected Cash Balance | 30/90 forecast events | Line with threshold (default slot 3) |
| Forecasting | Expected Inflows vs Outflows | Horizon summary | Grouped bars, 30 and 90 days |
| Forecasting | Projected Cash Shortfall Timeline | Dated cash curve | 90-day line, negative points in red |
| Forecasting | Baseline vs Scenario Forecast | Saved scenario | Insufficient Data with link to Decision Lab |

### Universal UI states (§7.7)

| State | Required behaviour | Where shown | Status |
| --- | --- | --- | --- |
| Loading | Skeletons, no misleading zero | Summary "Loading" state; import analysis | Covered |
| Empty | Add/Import actions; Not Provided vs Confirmed None | Summary "Empty" state; gallery | Covered |
| Insufficient Data | Name what is missing and how to add it | KPI cards, charts, YTD period, readiness | Covered |
| N/A | Undefined ratio, never 0 | Runway card, runway trend, gallery | Covered |
| Validation error | Field-level message; draft kept | Sign-up, setup, record modals, import review | Covered |
| Permission denied | Hidden controls; deep links rejected | Admin/Consultant on Summary; gallery deep link | Covered |
| Pending CEO approval | Waiting state; not shown as posted | Policy toggle on record save and import | Covered |
| AI unavailable | Manual paths and verified figures still work | Mapping, transaction form, insight card | Covered |
| Concurrency conflict | Show change; compare before retry | Gallery card only | Partial |
| Session expired | Re-auth; restore view without auto-commit | Gallery card only | Partial |
| Small screen / RTL | Mobile stacking, focus, Arabic | Onboarding and Summary at 390 px and in Arabic | Partial |

## Remaining decisions

22 decisions are open: 7 come straight from spec §10.4, and 15 are gaps this review found where the spec says "define", "proposed" or uses an asterisk without a rule. Owners are proposals, not accepted assignments; no entry changes the spec until the Product Owner answers.

| ID | Decision | Spec reference | Proposed owner | Resolve before | Current treatment in prototype |
| --- | --- | --- | --- | --- | --- |
| D-01 | How is a CEO-initiated action handled when policy requires CEO approval? | §5.3, §10.4, FR-APR-020 | Product Owner + backend lead | Approval-flow sign-off | Saves directly; labelled placeholder in the form |
| D-02 | Which non-CEO roles may edit official forecast assumptions? | §10.4, FR-FCT-016 | Product Owner + backend lead | Forecast page design | Accountant read-only (spec default) |
| D-03 | Retention periods for financial data, AI conversations and audit | §10.4, FR-AUD-010 | Product Owner + security/legal adviser | AI history and audit design | No retention or deletion promise shown |
| D-04 | Session technology, email-token lifetime, invitation expiry | §10.4 | Technical lead | Auth implementation | Verify screen shows no fixed durations |
| D-05 | Historical AR/AP snapshots without dated settlement events | §6.8, §10.4 | Product Owner + finance reviewer | Reports design | Aging shows current snapshot only |
| D-06 | Fully independent Decision Lab plans with no company baseline | §6.7, §10.4 | Product Owner | Decision Lab design | Not offered |
| D-07 | Is terms/consent acceptance required at sign-up? | FL-01 ("acceptance if required") | Product Owner + legal adviser | Sign-up sign-off | Labelled placeholder |
| D-08 | Do policy changes affect requests already pending? | §5.3, FR-APR-019 | Product Owner + backend lead | Policy editor design | Not shown |
| D-09 | Is alert acknowledge/resolve a stored state? | FR-ALT-013 ("proposed") | Product Owner | Alerts page design | Acknowledge button only; no stored state |
| D-10 | Are users notified when their access changes? | §5.1 (implementation proposal) | Product Owner | Permission editor design | Not shown |
| D-11 | Confirm the 22-subject chart catalogue | §7.3 ("proposed") | Product Owner | Chart library sign-off | All 22 implemented as listed |
| D-12 | Write the metrics-snapshot approach into the spec | Conversation decision; §10.4 | Product Owner + backend lead | Before API contract | Snapshot bar and "Updating metrics…" shown |
| D-13 | PDF export of reports | §1.4, §10.4 (not approved) | Product Owner | After MVP | Not offered |
| D-14 | Same-day event aggregation and end-of-day balance rule | §6.6 step 5 | Finance reviewer + backend lead | Forecast engine build | Assumes end-of-day balance |
| D-15 | Runway window: is "three completed months" the rule or only the fixture? | §6.4 | Finance reviewer | KPI directory design | Uses Aug–Oct 2026 |
| D-16 | Can users edit or delete their AI conversation history? | §4.2–§4.3 ("History\*") | Product Owner + security adviser | AI chat design | Not shown |
| D-17 | Archive vs delete for reports and tasks | §4.2 ("Archive\*"), FR-TSK-009 | Product Owner | Reports and tasks design | Not shown |
| D-18 | Member deactivation and reactivation rules | §4.2, §4.4 ("Deactivate\*") | Product Owner + backend lead | Team screen design | Not shown |
| D-19 | How does a user resolve a supplier bill and obligation that are one payment? | FR-AP-011, FR-OBL-013 | Product Owner + finance reviewer + backend lead | Obligations and forecast design | Note in supplier-invoice form only |
| D-20 | Does editing a recurring obligation change one occurrence, future ones or the series? | FR-OBL-005 (silent) | Product Owner + backend lead | Obligations design | Not shown |
| D-21 | What does the user see when an approved change fails to commit? | FR-APR-007, FR-APR-011 | Product Owner + backend lead | Approvals sign-off | Not shown |
| D-22 | Which dataset and scenarios demonstrate the pilot? | §6.9 (fixture only) | Product Owner + finance reviewer | Pilot acceptance testing | Oct 2026 fixture plus labelled demo history |

## Remaining UX refinement checklist

The next design rounds should follow this order: finish the record lists that the Summary already links to, then the analysis modules, then administration.

### Round 2 — Financial records

- [ ] Transactions list with search, filters, details drawer, edit and archive confirmation (FL-13).
- [ ] Customer invoices list with Unpaid / Partial / Paid / Overdue filters and paid-to-date update (FL-14).
- [ ] Supplier invoices list with due filters and the obligation-overlap warning, once D-19 is answered (FL-15).
- [ ] Obligations list with occurrence calendar and per-occurrence paid-to-date; series edit after D-20 (FL-16).
- [ ] Accounting adjustments list, generated-suggestion queue, draft/pending/confirmed states, useful-life request (FL-17).
- [ ] Record history drawer and archive confirmation shared by all five record types (BR-25).
- [ ] AI "Needs Review" state for unsafe or ambiguous classifications (FL-18).

### Round 3 — Analysis

- [ ] Standalone Forecast page: 30/90 toggle, dated event table, assumptions drawer, source drill-down (FL-21).
- [ ] Report Generator, coverage preview, 11-section Report Viewer, Saved Reports, Regenerate and compare (FL-20, FL-32).
- [ ] Founding date vs first-record date in all-available-history reports (FR-REP-018).
- [ ] KPI Directory listing all 33 KPIs with period/as-of selector (FL-31).
- [ ] Decision Lab: scenario list, line-item editor, results vs baseline, compare, share; outdated-baseline warning (FL-22).
- [ ] Ask FinPilot chat with history, evidence drawer, refusal, missing-evidence and tool-failure states, task proposal (FL-23).
- [ ] Alerts page with filters and email delivery status (FL-24).
- [ ] Tasks: create, assign, status change, history (FL-25).

### Round 4 — Administration

- [ ] Team list, invite form, pending invitations, cancel/resend (FL-11).
- [ ] Accept Invitation, expired invite, already-in-another-company states (FL-12).
- [ ] Role & Permission Editor with owner lock and sidebar preview (FL-28).
- [ ] My Permissions self-grant with audit note (FL-29).
- [ ] Approval Policy Editor with version and effect warning; approval history; stale and approved-but-not-applied states (FL-26, FL-27, D-21).
- [ ] Application configuration, financial settings (currency lock), alert thresholds (FL-30).
- [ ] Audit log list, filters and before/after detail (FL-30).
- [ ] 403 shown inside an already-open form after a permission change (FL-34).

### Cross-cutting

- [ ] Set-new-password and profile & security screens (FL-02).
- [ ] Granted read-only Consultant variant of Summary and Reports (US-041).
- [ ] Review Arabic copy, mixed-direction references (INV-2041, SB-1038), negative amounts and JOD precision with an Arabic reader.
- [ ] Specify keyboard order, focus return after modals and drawers, and screen-reader announcements for handoff.
- [ ] Run a moderated walkthrough with a representative CEO, Accountant, Admin and Consultant.

## Verification record

The prototype runs without script errors in English, Arabic and at 390 px width, and every §6.9 fixture figure it shows reconciles; runtime security and real calculations remain untested.

| Check | Result | Evidence / next step |
| --- | --- | --- |
| Script runs | Passed | Headless Chromium walk-through: sign-up → import → readiness → Summary, drawers, forms; no page errors. |
| Import validation | Passed | Fixing row 3 date and SB-1047 paid-to-date plus two duplicate decisions sets validated = true. |
| Atomic import failure | Shown | Simulated failure reports 0 records committed and keeps the draft. |
| Role sidebars | Matches §4.6 | All four roles switched via View as; Admin and Consultant denied on Summary. |
| Arabic RTL | Sampled | Onboarding and Summary mirrored; months use Arabic names with Latin digits. Other modules not yet designed. |
| Mobile | Sampled | Summary and sign-up at 390 px; no horizontal scroll after the grid fix. |
| Accessibility | Not certified | Labels, focus outlines, aria-live status and Esc to close exist; keyboard order and screen-reader pass still needed. |
| Backend rules | Not tested | Tenant isolation, 403 enforcement, idempotent commit and engine accuracy need implementation tests. |
| Design sign-off | Open | See decisions and checklist above. |

### Financial examples checked

| Measure | Arithmetic | Result (JOD) | Source |
| --- | --- | --- | --- |
| Net profit, Oct 2026 | 23,000 − 19,410 | 3,590 | §6.9 fixture |
| Gross profit | 23,000 − 7,360 direct costs | 15,640 | Fixture result; 7,360 derived |
| EBITDA | 4,240 EBIT + 600 D&A | 4,840 | Fixture result; D&A derived |
| Net cash flow, Oct 2026 | 14,800 − 17,150 | −2,350 | §6.9 fixture |
| Unpaid supplier bills | 1,600 overdue + 3,360 not yet due | 4,960 | Fixture total; split derived |
| Upcoming payments (#23) | 3,360 bills + 2,200 rent + 8,500 payroll + 750 loan | 14,810 | Matches fixture |
| Expected outflows, 30 days (#28) | 14,810 + 1,600 overdue bill | 16,410 | Matches fixture |
| 30-day closing cash | 5,220 + 20,200 inflows − 16,410 | 9,010 | Fixture result; 20,200 derived |
| 30-day low point | 5,220 − 1,600 − 2,200 − 500 | 920 on 10 Nov 2026 | Matches fixture |
| Cash runway | 5,220 ÷ 950 average burn (Aug–Oct) | ≈ 5.5 months | Demo history, not fixture |

These checks show the displayed numbers are internally consistent; they do not validate a forecasting engine. Figures marked "derived" or "demo" were added to complete the prototype and are not in the spec.

## Walkthrough script and exit checklist

Run these ten tasks with representative users on the prototype; record what happened, where they needed help and what to change, and never mark a task passed only because its expected outcome is written here.

| # | Role | Task | Expected outcome | Flows |
| --- | --- | --- | --- | --- |
| 1 | CEO | Register, verify (also try an expired link), create the workspace | Same message for new and existing emails; lands on Financial Setup | FL-01, FL-03 |
| 2 | CEO | Set up finance with "I don't know this yet" for opening cash | D1 shows Needs Attention; Current Cash shows Insufficient Data, never 0 | FL-04, FL-10 |
| 3 | CEO | Upload the three CSVs, resolve the Dr/Cr question, fix two row errors, decide both duplicates | Continue stays disabled until everything is resolved and validated | FL-05–FL-07 |
| 4 | CEO | Commit once with simulated failure, then for real | First: 0 records and draft kept; second: all records plus "Updating metrics…" | FL-08 |
| 5 | CEO | Leave obligations Not Provided, then Confirm None; go to Summary | Upcoming Payments changes from Insufficient Data to a value; no Finish Setup screen | FL-09, FL-10 |
| 6 | CEO | Explain why net profit is positive but net cash flow negative | Uses the KPI drawers and AI "How calculated" | FL-19, FL-31 |
| 7 | CEO | Swap chart slot 2 for Upcoming Payments Breakdown, reload, reset | Choice persists for this user; Reset restores the three defaults | FL-19, FL-33 |
| 8 | Accountant | With the approval policy on, add a transaction | "Pending CEO approval"; cash unchanged | FL-26 |
| 9 | CEO | Approve one request and reject another | Approve recalculates once; reject has no financial effect | FL-26 |
| 10 | Admin, Consultant | Open the Summary directly | Access denied with no figures revealed; Admin pointed to My Permissions | FL-29, FL-34 |

Repeat tasks 1–5 in Arabic and on a phone-width screen.

### Exit checklist

- [x] Prototype inspected against FP-MASTER-FR-001 v1.0.
- [x] All 51 business rules, 20 modules, 34 flows and 46 stories mapped to evidence.
- [x] All 33 KPIs, 22 chart subjects and 11 UI states mapped.
- [x] Open decisions recorded with proposed owners and timing.
- [ ] Product Owner answers D-01, D-02, D-12, D-19 and D-20 (they block round 2–3 screens).
- [ ] Rounds 2–4 designed and added to the prototype.
- [ ] All critical flows reviewed in Arabic and at phone width.
- [ ] Moderated walkthrough completed and findings fixed.
- [ ] Product Owner accepts the design baseline.

## Screen and route inventory

The prototype holds 17 designed screens and 18 route stubs, mapped to the §7.1 route registry; use "Prototype controls → Jump to screen" to open any of them.

| Screen | Route (§7.1) | Spec | In prototype |
| --- | --- | --- | --- |
| Landing | / | FR-AUTH-001 | Designed |
| Register Your Business | /auth/register | FL-01 | Designed |
| Verify Email (sent / expired) | /auth/register | FL-01 | Designed |
| Log In | /auth/login | FL-02 | Designed |
| Forgot Password | /auth/reset | FL-02 | Designed |
| Create Company Workspace | /onboarding/company | FL-03 | Designed |
| Financial Setup | /onboarding/finance | FL-04 | Designed |
| Choose Data Method | /onboarding/finance | FL-05, FL-09 | Designed |
| CSV Import wizard (5 steps) | /onboarding/import | FL-05–FL-08 | Designed |
| Start From Scratch hub | /onboarding/finance | FL-09 | Designed |
| Financial Readiness | /onboarding/readiness | FL-10 | Designed |
| Summary | /summary | FL-19 | Designed |
| All Financial Charts | /reports/charts | FL-33 | Designed |
| Record modals (5 types) | /financial-records/\* | FL-13–FL-17 | Designed (create only) |
| KPI, alert, notification and approval drawers | — | FL-19, FL-24, FL-26, FL-31 | Designed |
| Access denied | any protected route | FL-34, §5.5 | Designed |
| UI States Gallery | prototype only | §7.7 | Designed |
| Transactions | /financial-records/transactions | FL-13 | Stub |
| Customer Invoices | /financial-records/customer-invoices | FL-14 | Stub |
| Supplier Invoices | /financial-records/supplier-invoices | FL-15 | Stub |
| Financial Obligations | /financial-records/obligations | FL-16 | Stub |
| Accounting Adjustments | /financial-records/adjustments | FL-17 | Stub |
| Financial Reports | /reports | FL-20, FL-32 | Stub |
| KPI Directory | /reports/kpis | FL-31 | Stub |
| Cash-Flow Forecast | /forecast | FL-21 | Stub |
| Decision Lab | /decision-lab | FL-22 | Stub |
| Ask FinPilot AI | /advisor | FL-23 | Stub |
| Alerts | /alerts | FL-24 | Stub |
| Tasks & Approvals | /tasks-approvals | FL-25–FL-26 | Stub |
| Team & Access | /admin/team | FL-11, FL-28 | Stub |
| My Permissions | /admin/team | FL-29 | Stub |
| Application Configuration | /admin/configuration | FL-30 | Stub |
| Approval Policies | /admin/approval-policies | FL-27 | Stub |
| Audit Logs | /admin/audit | FL-30 | Stub |
| My Profile & Security | /profile | FL-02 | Stub |
