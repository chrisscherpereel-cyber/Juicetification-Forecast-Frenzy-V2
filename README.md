# 🥤 Juicetification: Forecast Frenzy

**A guided, hands-on forecasting lab for introductory Operations Management — Version 1.0**

Students step into the role of manager of *Juicetification*, the campus juice bar in the
student union. Over about 60 minutes they learn and **apply** every core demand-forecasting
method — computing each one by hand, writing the matching Excel formula, and then using their
best forecast to staff and stock the bar for a day of business. The lab closes with a
structured debrief and a submittable PDF report.

Each student receives a **unique scenario** (a random Session ID), so no two students work the
same numbers, yet every answer is auto-checked.

---

## Learning objectives

By the end of the lab a student can:

1. Explain what forecasting is and why service-sector demand is *perishable*.
2. Use qualitative signals (manager judgment, customer comments, known events).
3. Compute **naïve**, **moving-average**, and **exponential-smoothing** forecasts.
4. Build and apply day-of-week **seasonal indices** (computing the day and grand averages).
5. Forecast from drivers with a **regression** equation.
6. Measure accuracy with **MAD** and **MAPE**.
7. Read hold-out errors, identify the best model, and defend the selection.
8. Write the **Excel formula** for every method using proper cell references.
9. Turn a forecast into an operating plan and feel the dollar cost of forecast error.

---

## What's in the box

| File | Purpose |
|---|---|
| `forecast_frenzy.py` | The complete single-file Streamlit application. |
| `manifest.py` | The app's parameter schema for the Juicetification Director. |
| `juice_director.py` | Shared Director config loader (identical across every simulation repo). |
| `student_store.py` | Shared per-student progress store (identical across every simulation repo). |
| `README.md` | This file. |
| `Juicetification_Forecast_Frenzy_Quickstart.pdf` | One-page printable student quick-start sheet. |
| `Juicetification_Forecast_Frenzy_Quickstart.html` | Editable source of the quick-start sheet. |
| `Juicetification_Forecast_Frenzy_Development_Paper.docx` | Academic paper on the design and theory of the simulation. |
| `requirements.txt` | Python dependencies. |

The **Excel practice workbook** and the **submission PDF** are generated *inside* the app and
downloaded by the student — nothing to distribute separately.

---

## Requirements

- Python 3.9+
- `streamlit`, `pandas`, `numpy`, `openpyxl`

The PDF export uses a built-in, dependency-free writer — no extra packages required.

---

## Quick start

```bash
pip install streamlit pandas numpy openpyxl
streamlit run forecast_frenzy.py
```

The app opens in your browser. Students work left-to-right through the tabs; everything they
enter is saved automatically for the session.

---

## Instructor configuration (Juicetification Director)

Run without any URL parameters, the app uses its built-in defaults and behaves exactly as
described here. It also plugs into the Juicetification Director so an instructor can vary the
economics and demand without editing code. This is handled by two files, `manifest.py` (this
app's parameter schema) and `juice_director.py` (a loader that is identical in every simulation
repo, so it never needs editing).

Supported URL parameters:

- `?manifest=1` returns this app's parameter schema as JSON, so the Director can discover it.
- `?cfg=<base64 json>` applies a self-contained configuration (for example a different price or
  base demand). Values are type-checked and clamped to the limits in `manifest.py`.
- `?game=<code>` looks up a stored configuration through an optional shared endpoint.
- `?seed=<int>` fixes the scenario. A fixed seed gives an entire class the same data, while the
  default (no seed) gives each student a unique Session ID.

Configurable parameters include price, variable cost, fruit and bottle costs, wage, shift hours,
service rate, promotion cost and lift, the stockout penalty, and base daily demand. Base daily
demand also shapes the generated demand series, so it is included in the caching key to keep
different class configurations independent.

---

## Progress persistence and resume (optional)

When the storage secrets are configured, `student_store.py` persists each student's progress so
they can leave and resume, and each student receives a stable, unique scenario derived from their
student id. With a student id present in the URL (`?sid=`), a refresh automatically restores
state, and a completion record is written when the lab is finished. When the secrets are not set,
every storage call is a safe no-op and the app behaves exactly as it does by default.

Configuration lives in Streamlit secrets or environment variables: `DB_ENCRYPTION_KEY` plus
either `DROPBOX_REFRESH_TOKEN` with `DROPBOX_APP_KEY` and `DROPBOX_APP_SECRET`, or
`DROPBOX_ACCESS_TOKEN`. The `dropbox` and `cryptography` packages in `requirements.txt` are
required once storage is enabled.

---

## How the lab flows

The lab is organized as a set of tabs. Every learning module follows the same rhythm:

> **Objective → Formula → *You apply it* → *Write the Excel formula* → Feedback → Guiding question → Apply it again**

**Tabs, in order:**

1. **📖 Start Here** — the business, the "perishable capacity" idea, and a warm-up.
2. **Modules 1–2** — what forecasting is; qualitative forecasting from a given briefing.
3. **Modules 3–5** — naïve, moving average, and exponential smoothing (with guided
   exploration of window width and the smoothing constant α).
4. **Module 6** — seasonality: students compute the **day average** and **overall (grand)
   average** themselves, then form and apply the index.
5. **Module 7** — regression from temperature, promotions, and attendance.
6. **Module 8 — Accuracy** — compute MAD and MAPE (placed *before* model selection on purpose).
7. **Module 9 — Model selection** — read the hold-out error table, identify the lowest error,
   and select a method to carry forward.
8. **🏪 Run *Juicetification*** — choose a forecast → calculate the staffing/prep plan → implement
   it → open for the day and see the P&L.
9. **🎓 Debrief** — a structured *What? / So what? / Now what?* that surfaces the student's own
   biggest forecast miss, its dollar cost, and one rule to carry into a real operation.
10. **📝 Final Report** — review checklist, scores, and the downloadable submission PDF.

### Applying the formulas

For every calculation the student chooses how hands-on to be — *compute it myself* (checked with
worked solution), *estimate then reveal*, or *show answer and interpret*. They then write the
**Excel formula**, which the app actually evaluates against the on-screen grid. Formulas must use
**cell references**; typing a value that appears in the grid is rejected, so students build real
spreadsheet skills.

---

## How students are scored

Three scores are reported **separately**, so effort never hides a shaky calculation:

- **Completeness /100** — how much of the lab was finished (effort).
- **Mastery /100** — share of calculations and Excel formulas correct on the **first try** (skill).
- **Performance /100** — how well the operating plan matched demand while running *Juicetification*
  (application: 60% profit vs. a perfectly-matched plan, 40% customer satisfaction).

Written reflections use lightweight **self-rubrics** (e.g., *names a cause, names a trade-off,
connects to a decision*) so the qualitative work gets formative structure too.

---

## Submission

On the **Final Report** tab the student reviews a completeness checklist (and can revisit any tab
to edit), then downloads a **submission PDF** containing their name, Session ID, selected method,
all three scores, and every answer, Excel formula, and reflection. Markdown and CSV exports are
also available.

---

## For facilitators

- **Reproducible scenarios.** Each student's data is keyed to their Session ID, shown on the PDF —
  useful for spot-checking or regenerating a student's exact numbers.
- **Grading.** The PDF captures reasoning and marks calculations correct/incorrect; the CSV export
  gives a quick machine-readable record. The three scores separate effort, skill, and application.
- **Time.** Budget ~60 minutes; the per-module objective boxes carry suggested minutes.
- **Configuration.** Cost and demand assumptions live in clearly labeled constants near the top of
  `forecast_frenzy.py` (price, variable cost, wage, service rate, promo economics, day-of-week
  seasonality). Adjust them to match your course context.

---

## Design notes

- **Unique-but-checkable data.** Scenario numbers are randomized per session yet rounded so hand
  calculations stay clean and answers remain auto-gradable.
- **Feedback after input.** Evaluative feedback appears only after the student acts — no
  answers or verdicts are shown before a response.
- **Experiential arc.** Concrete decision (run the bar) → reflective debrief → an abstract rule the
  student commits to carrying forward, mirroring the experiential-learning cycle.

---

## Version

**V1.0** — first publication release.

*All figures in the simulation are illustrative and for educational use.*
