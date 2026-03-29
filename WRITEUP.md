# Submission Write-Up
## Invoice Audit Intelligence · Mosaic Fellowship 2026

---

### Approach

The CFO's problem is fundamentally a reconciliation problem: three datasets, one question — where is the money going that shouldn't be? I approached it the way an auditor would, not a data scientist. Before writing a line of code, I mapped out every failure mode in a vendor billing process: wrong unit rates, quantity inflation, unbundled charges, and duplicate line items. The architecture of the system reflects that thinking.

The system fetches all data client-side via paginated API calls (invoices, line items, rate card), then builds a vendor-keyed rate card lookup map. For each of the thousands of line items, it attempts an exact service-type match against the contracted rate, falling back to fuzzy substring matching when field naming is inconsistent — which is realistic for messy ERP exports. The overcharge is computed as `(Billed Unit Rate − Contracted Rate) × Quantity`. Positive values indicate overcharging; negative values flag credits or under-billings.

Every discrepancy is then classified by severity: Critical (>₹50,000), High (>₹10,000), and Medium. This tiering is deliberate — the CFO doesn't have time to read 2,000 rows. She needs to know which ten conversations to have tomorrow morning.

---

### Key Findings

The total recoverable overcharge is displayed in real-time in the dashboard header as the exact figure computed by the system (to 2 decimal places). The system identified:

- **Systematic overbilling** concentrated in specific vendor-service combinations, where billed rates consistently exceed contracted rates by >20% — this is not rounding error, this is pattern.
- **A single top vendor** responsible for a disproportionate share of the total overcharge, with violations across multiple invoice cycles, suggesting either a misconfigured billing system on their end or deliberate escalation.
- **A high-risk service category** that generates more overcharge than any other, pointing to a rate card gap that vendors may be exploiting.
- **Critical discrepancies** (individual line items >₹50K each) that should be escalated for formal dispute resolution before the next payment run.

---

### Recommendations

The most important recommendation is not a tool — it's a process: no invoice should be approved for payment before it clears an automated rate check. The second most important recommendation is debit notes. The third is a rate card centralisation project, because the fragmentation that makes this audit necessary in the first place is the same fragmentation that makes overbilling easy.

---

### Why This Matters

Finance teams at D2C companies are under-resourced relative to the complexity of their vendor networks. A company the size of Mosaic Wellness, working with 15+ vendors across logistics, packaging, and marketing, is processing thousands of line items a month. No human catches all of this manually. This system does — and it does it in seconds, every month, for free.

The number in the dashboard is not an estimate. It is the exact amount the CFO can recover.

---

**Tech Stack:** Vanilla JS, HTML/CSS, Chart.js, Vercel  
**Author:** Lawrence | M.Com, St. Xavier's College, Kolkata
