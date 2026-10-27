# Landing

**Losing your GLP-1 coverage? Land safely.**

Landing helps people whose GLP-1 coverage is ending, or who have already stopped, see what staying on would really cost, prepare a decision with their prescriber, and keep the progress they made if they stop.

Built for Babson's *The Ultimate Entrepreneurial Challenge* (Prof. Len Green), GLP-1 opportunities session, October 28, 2026.

## The problem

- 11% of US adults currently take a GLP-1 for weight loss, up from 3% in 2024. (Gallup, July 2026, n=5,065)
- 49% of people who started semaglutide for obesity stopped within a year. (Mailhac et al., *JAMA Network Open*, Aug 2026, n=157,822)
- About 60% of the weight lost is regained within a year of stopping. (Budini et al., *eClinicalMedicine*, Mar 2026, DOI 10.1016/j.eclinm.2026.103796)
- Large employers covering GLP-1s for obesity fell from 72% in 2025 to 60% in 2026, and 14% have dropped or plan to drop coverage by 2027. (Business Group on Health, via SHRM)
- Cost or insurance was the top reason for stopping (47.6%) in a Cleveland Clinic cohort of 288 people who stopped.

## Who it's for and how it's different

| | Before the last dose | After stopping | Who pays |
|---|---|---|---|
| **Landing** | Coverage countdown, self-pay price comparison, transition checklist, questions for the prescriber | Hold-zone tracking, plan, coach, photos | Consumer first, then employers as a transition benefit |
| Noom GLP-1 Companion (free) | For people taking the drug | Partly | Consumer |
| Virta | | Off-ramp program | Employer only |
| Telehealth (Ro, Hims, Found, Calibrate) | Sells the drug | No | Consumer |

Landing is neutral. It shows published prices and leaves the medical decision to the user and their prescriber.

## What's in the MVP (`index.html`)

- **Onboarding:** asks whether coverage is ending or the user has already stopped, then medication, dates, weights, and training level.
- **Before the last dose:** a countdown to the coverage end date, a 6-step transition checklist, and a "Start my landing" button for the day of the last dose.
- **Coverage page:** self-pay prices for Wegovy injection, Wegovy pill, Zepbound, and Foundayo by dose, plus copyable questions for the prescriber.
- **Overview after stopping:** a status (Holding steady, Drifting, or Reset recommended), a weight chart with hold and drift zones against the typical regain path, and a Today checklist.
- **Progress photos:** stored only on the device in IndexedDB. Includes a snapshot linked to chart dates and a side-by-side compare view.
- **Plan:** a protein range, a 3-day strength program, habits, and an automatic one-week reset when the user is drifting.
- **Coach:** an AI coach that won't give advice on doses, tapering, or restarting medication.
- **Weigh-ins:** a history table with weekly changes and photo thumbnails.
- **Savings finder (Coverage page):** two questions (denial type, HSA/FSA) produce an estimated yearly savings range, a lower-cost option check, and a draft appeal or exception letter for the prescriber to complete.
- **Landing guarantee (pilot terms, not live):** shows the share of weight loss kept against a 75%-for-12-months goal.
- **Dietitian escalation (demo):** when a member drifts, offers a 15-minute call with time slots. No request is actually sent.
- **Employer dashboard (sample data):** affected employees, enrollment, share in hold zone, member decisions, an illustrative cost comparison, and a communication kit (announcement email, FAQ, manager talking points). Aggregates only.

Three demos are available from the onboarding screen: "Coverage ends in 3 weeks," "9 weeks after stopping," and "Employer dashboard."

## Running it

It's a single static file with no build step.

```
open index.html          # or: python3 -m http.server, then visit localhost:8000
```

**Platform note:** The live version runs as a Claude artifact. Two features use Claude's artifact runtime (`window.claude`):

| Feature | On claude.ai | Run locally or on GitHub Pages |
|---|---|---|
| Weigh-ins and profile | Saved to the user's account (`db` capability) | Falls back to `localStorage`, so data stays in that browser |
| AI coach | Works (`sample` capability) | Shows "not available." Replace `askCoach()` with a call to your own backend that calls an LLM API. Never put an API key in the front end. |
| Photos | Stored on the device | Stored on the device |

Look for `boot()`, `save()` and `askCoach()` in the script to plug in a real backend.

## Prices (Coverage page)

The prices are hard-coded in the `PRICES` array, checked in October 2026 from secondary sources (OneDayMD, Oct 18, 2026; Affinity Whole Health). **Verify them against NovoCare and LillyDirect before any real use.** Several offers end December 31, 2026.

## Guardrails

- Never advise on doses, tapering, switching, or restarting a GLP-1. Those questions go to the prescriber.
- The coach avoids very-low-calorie diets and fasting. If someone describes disordered eating, it points them to a clinician and the National Alliance for Eating Disorders helpline.
- Photos are never uploaded.

## Open to-do list

- [ ] Verify all prices with the manufacturers. Add an "as of" date that's checked automatically.
- [ ] Real backend: authentication, a database, and a server-side LLM call for the coach.
- [ ] Opt-in encrypted photo backup.
- [ ] Weekly weigh-in reminders by email or SMS.
- [ ] Employer admin view: invite affected employees, see anonymized aggregate engagement.
- [ ] Units toggle (lb and kg).
- [ ] 90-day pilot: recruit 20 people who recently stopped or are losing coverage (r/Semaglutide, r/Zepbound). Measure weekly retention, the share in their hold zone at 12 weeks, and willingness to pay.

## Evidence notes for the pitch

- No trial has tested a program that starts *after* stopping. The closest is S-LiTE (*eClinicalMedicine*, 2024): people who exercised during liraglutide treatment regained 6.0 kg less in the year after stopping than people who took the drug alone. The pilot is designed to produce this evidence.
- About 36–47% of people who stop restart within a year. Landing's Coverage page serves people deciding whether to restart, and the landing plan serves those who stay off.
- Claims that come only from secondary sources: Virta's roughly $900/year price, and the Starbucks, Allina, and PwC coverage changes.

*Not medical advice. Prototype only.*
