# BREDA SWCS — Applicant Module (Wireframes & Hi-Fi)

Single Window Clearance System for BREDA (Bihar Renewable Energy Development Agency),
Govt. of Bihar. This project holds the **applicant-facing** module designs.

## Source of truth
- **SRS:** `uploads/BREDA_SWCS_SRS_v1.2 (1).md` — read this first. Module refs: M06 = Applicant
  Dashboard, M07 = Status Tracker. Always cite/align to the SRS.
- Reference screenshot of the desired top-bar/sidebar layout: `uploads/pasted-1781933205637-0.png`

## Files (all hi-fi unless noted)
- `Applicant Dashboard.dc.html` — M06, "Action-first" direction. CTAs wired to Review Page + Track.
- `Application Track.dc.html` — M07 status tracker (stepper + queries + timeline). Amber banner CTA → Review Page.
- `BREDA Review Page.dc.html` — `/review/$id`, officer + applicant in one file, role toggle in top bar. **Drawer pattern:** compact rows with textual action links (💬 raise / Respond → / View history →); clicking opens a right-side slide-in drawer with the field's full Original→query→response **revision-history timeline** (replaces the old inline accordion AND the standalone Review Diff screen). Officer decision (Approve/Reject) lives in the right rail.
- `BREDA Officer Entry Point.dc.html` — BREDA Officer applications queue (was `Dep Officer Entry Point.dc.html`, renamed June 2026). Tabbed (Needs Action / All / Closed), stats strip, textual row CTAs → BREDA Review Page. Tweaks: defaultTab, showStats, compactRows.
- `Dept Officer Entry Point.dc.html` / `Dept Officer Review.dc.html` — Dept. Officer clearance queue + review. Row CTAs → Dept Officer Review.
- `Commissioning.dc.html` — M10 COD lifecycle. Top-bar role toggle (Applicant ↔ BREDA Admin). Applicant `stage` tweak: cod_pending (enter COD + upload cert) → under_verification → cod_rejected (query + re-upload) → registered (Reg No. `BREDA/SPV/Power Plant/<seq>/<year>` + Appendix-D certificate). Admin: COD verification queue (Verify generates Reg No. / Raise query returns it). Deep-link `Commissioning.dc.html#registered` opens the issued certificate. Tweaks: role, stage, showStats.
- `Payments.dc.html` — M? Payments + Finance verification. Top-bar role toggle (Applicant ↔ Finance). Applicant: registration-fee table per project + stats. Finance: offline-challan verification queue with working Verify/Reject (state). Tweaks: role, showStats. Receipt/View-proof/Download-statement → `#`.
- `Apply for Clearances.dc.html` — departmental clearance basket (7 depts). Each service = checkbox row; expanding a checked row shows supporting-doc + fee-challan upload **except the two BSPTCL rows**, where the statutory form replaces it (FORMAT-2 Section F + Enclosure 1 already carry the fee) — those rows are gated by form completion alone, the other six by challan/receipt. **BSPTCL statutory forms live in-row, as tabs** (built Sept 2026, from `uploads/BSPTCL, STU Clearance-pages.pdf`):
  - **Grid Connectivity → FORMAT-2** (application for grant of connectivity). Tabs: Applicability · Applicant · Connectivity · Existing · Project · Activities · Fees · Undertakings.
  - **Final Grid Connectivity Approval → FORMAT-4** (additional info for signing the connection agreement). Tabs: Applicability · Applicant · Maps (Sch. I–III) · Connection · Communication · Sch. IV · Sch. V · Sch. VI · Sch. VII · Sch. VIII · Certification. The five schedules are separate tabs and appear only for a generating plant.
  - Both formats branch on **applicant status** (generator / captive generator / distribution licensee / captive user / OA consumer): Section C is dropped for a distribution licensee; Section D has four statutory branches; FORMAT-4 also branches applicant ↔ intra-State transmission licensee, and derives the **two-month acceptance deadline** from the FORMAT-3 offer date.
  - No Preview tab (removed by user request) — the completion gate (confirm checkbox + “Mark form as complete”) sits on each form’s last section. Tab dots show per-section completeness. A clearance with a form **cannot be submitted until the form is marked complete** (sticky-bar warning names the pending form).
- Two more in-row forms (Sept 2026):
  - **EIO → Charging Permission → Work Completion Report** (`uploads/Work Completion Report Electrical Inspector (2)-pages.pdf`). Tabs: Covering letter · A PV Module · B Inverter · C–F Cables & boxes · G–H Earthing & signs · I–J Declaration · Work Completion Certificate. Total KWp auto-computes from module capacity × count; earth resistance ≥ 5 Ω raises an amber hint (Rule 47A limit). It has its own signature block (applicant + installer firm / supervisor / contractor), so it does NOT use the generic Place/Date/Signatory block.
  - **PHED → NOC for Canal / Reservoir / Pond → Water Resources NOC**. Tabs: Project & site · Water body · Declaration. Client-specified fields: District, Project Location, Name of river, Name of canal. A water-body selector (Canal / Reservoir / Pond / River) makes river or canal name mandatory as appropriate.
  - **Fee card and form are independent flags.** `noFee:true` (BSPTCL only) hides the supporting-doc/challan card; `form:'<KIND>'` attaches a statutory form. All four form-bearing rows currently set `noFee:true`, so the challan card only shows on the five rows without a form. Submit gate = (proof unless `noFee`) AND (form done if `form`).
  - Adding a form: set `form:'<KIND>'` on the service, add a `newForm` branch, a `FORM_META` entry, `xxxTabs()` / `xxxGroups()` methods, and register them in `tabsFor` / `groupsForKind` / `signTabFor`. `signTabFor` returns null for formats with a bespoke signature block. Tab fallback is `ids[0]`, so a form need not have a tab called `T`. Form state is read through `fm(id)` / `_f(st,id)`, which fall back to a fresh `newForm()` — this keeps the page from crashing on a hot reload whose preserved state predates a newly added form.
  - Fields carried over from the BREDA application are shown with a green tint (no chip label — removed by user request). Section renderer is fully data-driven (`G/H/FLD/NOTE/RADIO/F/RF/UNITS/ACTS/DOCS/UND/TBL` builders) — add sections in the script, not the markup.
- `Applicant Dashboard Wireframes.dc.html` — **low-fi** board, 3 layouts (A/B/C). A chosen.
- `Review Page Wireframes.dc.html` — **low-fi** board, officer + applicant modes. Superseded by hi-fi.

## Design system (keep consistent across ALL files)
- **Fonts:** IBM Plex Sans (UI), IBM Plex Serif (headings/H1). Wireframes use Caveat + Patrick Hand.
- **Sidebar:** dark green `#0f2319`, width 252px. Logo = BREDA icon + "BREDA SWCS" / "Applicant Portal".
  Active nav item = amber left-border `#f59e0b` + `rgba(255,255,255,.07)` highlight.
  Nav order: Dashboard · New Project Application · **My Applications** · Payments · Notifications (badge) ·
  Help & Grievance · Profile.
- **Top bar (56px, white):** page title left; right side = `DEMO BUILD` pill + `DEMO ROLE` label +
  role switcher (Applicant · BREDA Officer · Dept. Officer · Finance) + notification bell (badge) + RK avatar.
- **Page header (white, below top bar):** breadcrumb + H1 (Plex Serif) + primary CTA on one row.
- **Primary green:** `#1e9b5f`. **Page bg:** `#eef1ef`. **Cards:** white, 14px radius, 1px `#e3e9e6`, soft shadow.
- **Status color mapping:**
  - Attention / query / payment-pending → amber. Pill `#fef3e3`/`#f0d6ac`/`#92400e`, dot `#c2740f`. Action buttons `#e05c1a`.
  - In progress / under review → blue. Pill `#e6f0f9`/`#c9dff2`/`#1a5a96`, dot `#1d6fb8`.
  - Waiting / verification → slate grey `#6b7c72`.
  - Done / approved → green `#1e9b5f`.

## Removed by user request (do NOT add back)
- No GIGW utility bar (Govt of Bihar strip, text-size, high-contrast, language toggle).
- No footer link strip (Sitemap / Website Policy / Accessibility Statement / Help / Feedback).
- No accessibility controls anywhere for now.

## Sample data — keep stable for continuity
Applicant: **Rajesh Kumar** (avatar initials RK). Three projects:
1. **Solar Rooftop — Patna** · 500 kW · Ack/2025/014 · **BREDA Query Raised** (2 queries: Nearest PSS, DPR doc). Round 2 in progress.
2. **Ground-Mounted Solar — Gaya** · 5 MW · Ack/2025/009 · Under Dept. Review (3 of 6 clearances done).
3. **Solar + Storage — Bhagalpur** · 12 MW · Ack pending · Payment Verification.

## Full officer flow (all screens connected)
```
Officer Entry Point → Review Page (officer mode, raise queries → Send)
  → applicant sees Review Page (applicant mode, opens drawer per field, responds → Resubmit)
  → officer opens each changed field's drawer (Original→Corrected timeline)
  → Approve / Reject / Request Further Revision (right rail) → Review Page round 2
```
(Review Diff retired June 2026 — the per-field drawer timeline covers the diff; row badges flag every changed field at a glance.)

## Agreed design decisions (do not re-litigate)
- Review page: single scrollable document, NOT a stepper. Role toggle in top bar.
- Officer 💬 on every field; queries are drafts until "Send". General query at bottom (discouraged).
- Review Page uses a **slide-in drawer** (not inline accordion): officer composes/sends queries and views history in the drawer; applicant responds in the drawer (text field / address sub-inputs / doc re-upload). "Save draft correction" → sticky "Review & Resubmit" bar unlocks when all queries answered → creates new version → `RESUBMITTED_TO_BREDA`.
- Per-section grid: all rows in a section with any query share one column template (`1.3fr 1.5fr 150px`) so the value column stays vertically aligned; query-free sections use `1.3fr 1fr`.
- Diff is shown **inside the field drawer's revision-history timeline** (Original v1 → query → response v2…, version chips). No standalone diff screen.
- Officer entry point: tabular list (not cards), textual CTAs (no button backgrounds), left-border accent per status row.

## What's next to build
1. ✅ **My Applications page** (`My Applications.dc.html`) — DONE. Tabular list of all 12 projects, tabs (All · Action Needed · In Progress · Completed), stats strip, individual row actions. All status types covered (Draft, BREDA/Dept Query, BREDA Approved, Payment Verification Pending, Submitted/Resubmitted, Approved, Rejected, COD Rejected, Awaiting COD). Respond → Review Page; Track → Application Track; Continue/Apply/Submit/Re-upload COD → `#` (targets not built).
2. ✅ **New Project Application form** (`New Project Application.dc.html`) — DONE. Reactive DC port of `uploads/new-application-form.html` (content/logic kept as-is, reskinned to design system + standard shell). 8-step wizard: Step 0 (capacity+purpose, auto PSS/GSS rule + fee slab) → A (profile/address, comm→proj copy) → B (technical: plant kind/subtype/storage, DISCOM→PSS, distance, evacuation, EPC) → C (land) → D (dynamic docs: GST cert if GST entered, PPA if PPA purpose; custom docs) → E (declaration, gated checkbox) → Preview (full readout, non-refundable confirm) → F (fee card, online/offline/demo pay → success screen w/ Ack no.). Stepper sidebar, autosave badge. Note: panel content data-driven for address/doc grids.
3. ✅ **Review Page drawer pattern** — DONE (June 2026). Inline accordion replaced by slide-in drawer + revision-history timeline; Review Diff retired. Per-section grid alignment fixed.

## NEXT PASS — queued (this chat got long; pick up fresh)
1. ✅ **"All Dept. clearances received" state** — DONE (June 2026). Added a 4th demo scenario to `Application Track.dc.html` ("Gaya · All Clear" tab / `scenario:'gaya_done'`): green success banner, stepper advanced to Stage 5 Commissioning, all 6 clearances Approved (100% bar), COD next-step card, completed timeline. Gaya block now renders for both `isGaya` and `isGayaDone` via `isGayaAny`; clearance header/bar/badge are data-driven. NOT yet reflected in Review Page (Dept. Officer) or the dashboard's Gaya row — pick up there if needed.
2. ✅ **Wire all navigation end-to-end** — DONE (June 2026). Applicant Dashboard sidebar + CTAs, My Applications draft "Continue" → New Project Application, Application Track / Review Page sidebars all now real `<a href>`. Fixed 2 Officer Entry Point rows that pointed at retired `Review Diff.dc.html` → `Review Page.dc.html`. Still `#`: COD targets in My Applications (Submit/Re-upload COD — needs COD lifecycle #4), Payments sidebar item on form pages, Notifications/Help/Profile everywhere.
3. ✅ **Payments + BREDA-level verification** — DONE (June 2026). New `Payments.dc.html`: role toggle in top bar (Applicant ↔ Finance; BREDA/Dept dimmed, like Review Page). **Applicant mode:** stats strip + registration-fee table per project (Patna ₹15k Verified · Gaya ₹25k Verified · Bhagalpur ₹50k Verification Pending · Nalanda ₹2,360 Payment Due); status tones per design system; Track→Application Track, Pay now→New Project Application. **Finance mode:** verification queue of offline challans with working Verify/Reject (updates row status + pending count in state). Tweaks: role, showStats. NOTE: receipt view, "View proof", Download statement → `#` (not built). Bhagalpur amount ₹50,000 / DD 418225 · SBI kept consistent with Application Track. (Payment instrument is **DD or Cheque** — never "challan"; the clearance-fee challan in Apply for Clearances is a separate counter-payment flow.)
4. **COD lifecycle** — Commissioning: COD submit → review → reject/re-upload states (ref: My Applications already has COD Rejected / Awaiting COD statuses). The Application Track `gaya_done` "Apply for Commissioning" CTA and My Applications Submit/Re-upload COD currently point at `#` — wire to the COD screens once built.
5. **Notifications panel** — bell badge = 4 unread.

### Files to reference for visual consistency
- `Review Page.dc.html` — drawer pattern, status colors, per-section grid alignment.
- `Officer Entry Point.dc.html` / `My Applications.dc.html` — tabular list + textual row CTAs.
- `Application Track.dc.html` — status stepper.
- `Applicant Dashboard.dc.html` — sidebar/topbar shell markup.
