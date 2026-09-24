# PetZen — Pet Parent app: v1 requirements

**Author:** Product manager / analyst ·

 **Date:** 19 September 2026 · 

**Status:** Draft for discussion


**Supersedes:** the CRM-first framing in `docs/product/feature-inventory.md`. Service providers move to Phase 2.

---

## 1. Vision, restated

One app that is with a person for the whole arc of having a pet: **deciding** whether to get one,
**learning** how to look after one, and **running** the life of one — records, reminders, and later
every service the pet needs, in one place.

Three audiences, but **one lifecycle**. A prospective parent who adopts becomes a new parent; a new
parent at month twelve becomes an existing parent. Build it as one app with stages, not three apps.

| Stage | Who | What the app does for them |
|---|---|---|
| **Wondering** | Thinking about a pet, not sure | A virtual pet that shows what a year of ownership is actually like |
| **Beginning** | Just adopted, or picked one up off the street | A guided Day 1 → Year 1 journey: what to expect, what to do, what not to do, and someone to ask |
| **Living** | Has a pet, wants it well looked after | The pet's record, reminders, and — later — every service booked from the profile |

---

## 2. The shared core: two things everything sits on

### 2.1 The Pet Profile

The persistent record of a real pet: identity, health, food, behaviour, people. Owned by the parent,
shared by consent. This is what every later service — hostel, sitter, walker, vet, shop — reads
from. It is the CRM's pet profile, promoted from "input to a booking" to "the centre of the product."

### 2.2 The Journey Engine

A content model: **species × life stage → what is happening, what to expect, what to do, what to
watch for.** Puppy at 8 weeks: second vaccination due, teething starts, crate training window,
socialisation before 14 weeks. Kitten at 6 months: neutering conversation, litter habits settling.

This one engine drives two experiences with **two different clocks**:

- **Beginning** stage — the engine runs on the **real clock**, against the real pet's actual age.
  "Your puppy is 10 weeks old. This week: …"
- **Wondering** stage — the engine runs on an **accelerated clock**, against a virtual pet.
  "Day 42 with Biscuit. Overnight: …"

Same content, two presentations. This is the single most important design decision in the product:
it means the virtual pet is not a separate game to build, it is the journey guide with a
compressed timeline and a character on top.

**Consequence:** the Journey Engine's *content* must exist before either the guide or the virtual
pet can. Content is on the critical path and is not software.

---

## 3. Product principles

1. **The parent owns the pet's data.** Everything is shareable by choice, nothing by default.
2. **The app informs; a vet diagnoses.** Guidance is general, reviewed, and always defers symptoms
   to a professional. This is a safety and liability line, drawn once and never crossed.
3. **Honest about ownership.** The virtual pet shows the whole year — the 6 a.m. walks and the vet
   bill as well as the cuddles. An app that only shows the bright side converts the people who
   return the pet at month three. Honest is kinder, and more credible. *(Open — see §7.)*
4. **Content is versioned like code.** Every piece of guidance has a source, a reviewer, a region,
   a species, and an age range. Nothing is published anonymously.
5. **Useful on day one, without any provider on the platform.** A parent must get value from the
   app alone. Providers are Phase 2.
6. **Dogs and cats first.** Content cost scales with species. Everything else is later.

---

## 4. Epics and stories (v1)

Stories are written as: *As a \<stage\>, I want … so that …*, with acceptance criteria as
observable behaviour. **Must / Should / Could** marks v1 priority.

### E1 — Account and household

| # | Story | Pri |
|---|---|---|
| 1.1 | As a parent, I can register with email and password, verify, and reset, so my data is mine. | Must |
| 1.2 | As a parent, I can have several pets under one account. | Must |
| 1.3 | As a parent, I can invite a co-parent to a pet with view or edit rights, so a household shares one record. | Should |
| 1.4 | As a parent, I can delete my account and receive an export first. | Should |

**Acceptance (1.3):** Given a co-parent with *view* rights, when they open the pet, they see the
profile and records but every edit control is absent and every edit request is rejected server-side.

### E2 — Pet Profile and records

| # | Story | Pri |
|---|---|---|
| 2.1 | As a parent, I can create a pet: name, species (dog/cat), breed or "mixed/unknown", sex, neutered, date of birth **or estimated age**, weight, photo. | Must |
| 2.2 | As a parent of a rescued pet, I can enter "roughly 3 months" and the app derives a working birth date and marks it estimated, so the journey still works. | Must |
| 2.3 | As a parent, I can record vaccinations: type, date given, next due, certificate photo; the app shows what is current, due, and overdue. | Must |
| 2.4 | As a parent, I can record parasite prevention (tick, flea, worm): product, date given, repeat interval; the app shows when the next is due. | Must |
| 2.5 | As a parent, I can record allergies and dietary restrictions, and the food the pet actually eats (brand, amount, times). | Must |
| 2.6 | As a parent, I can record my vet: name, practice, phone, address, and an out-of-hours number. | Must |
| 2.7 | As a parent, I can record medical history: conditions, medications (drug, dose, schedule), procedures, with documents. | Should |
| 2.8 | As a parent, I can describe temperament and behaviour with a structured set of flags plus free text, so a provider knows what they're getting. | Should |
| 2.9 | As a parent, I can log weight over time and see the trend. | Could |
| 2.10 | As a parent, I can store documents: microchip certificate, insurance, adoption papers, passport. | Should |

**Acceptance (2.3):** Given a vaccination with next-due 14 days away, the pet's health summary
shows it as *due soon* with the date; given next-due in the past, it shows *overdue* with the number
of days; the status is derived, never stored.
**Acceptance (2.2):** Given "about 3 months" entered on 19 Sep 2026, the profile stores an
estimated DOB of ~19 Jun 2026 flagged `estimated`, and every age-based feature uses it.

### E3 — Reminders

| # | Story | Pri |
|---|---|---|
| 3.1 | As a parent, I get a reminder before a vaccination or parasite treatment is due (configurable lead time). | Must |
| 3.2 | As a parent, I get medication reminders at the scheduled times, and can mark a dose given or skipped. | Should |
| 3.3 | As a parent, I can set custom reminders (grooming, vet visit, insurance renewal). | Should |
| 3.4 | As a parent, I choose channels — push, email — and quiet hours. | Must |
| 3.5 | As a parent, when I mark a treatment as given, the next reminder is created automatically from the repeat interval. | Must |

**Acceptance (3.5):** Given monthly tick prevention given on 1 Oct, when marked given, a new
due-date of 1 Nov exists and a reminder is scheduled for the configured lead time before it; marking
it given twice does not create two.

### E4 — Journey Engine (content platform)

| # | Story | Pri |
|---|---|---|
| 4.1 | As the product, I hold journey content structured as *species → life stage (age range) → topics*: what's happening, what to expect, what to do, what not to do, when to see a vet. | Must |
| 4.2 | As a content author, every item carries species, age range, region, source, reviewer, review date and version, and is unpublished until reviewed. | Must |
| 4.3 | As the product, I can resolve "this pet, today" to the right set of content, using real or estimated age. | Must |
| 4.4 | As the product, I can resolve "this virtual pet, day N" to the same content on an accelerated clock. | Must (for E7) |
| 4.5 | As a content author, I can edit and republish content without a code deploy. | Should |

**Acceptance (4.3):** Given a dog with estimated DOB 14 weeks ago and region UK, the resolved
content includes the 12–16 week stage for dogs/UK and nothing from other stages or regions.

### E5 — Beginning: the new-parent guide

| # | Story | Pri |
|---|---|---|
| 5.1 | As a new parent, after entering species and approximate age, I see "where you are" and "what's next" — this week's milestones, tasks and warnings. | Must |
| 5.2 | As a new parent, I see the full Day 1 → Year 1 timeline for my pet, with the current point marked. | Must |
| 5.3 | As a new parent, I get a "first 72 hours" checklist on day one (food, water, safe space, vet registration, ID). | Must |
| 5.4 | As a new parent, I see the vaccination schedule for my species and region, pre-populated as *expected*, which I confirm as each is given. | Must |
| 5.5 | As a new parent, I get behaviour explainers keyed to age — teething, biting, zoomies, night crying, spraying — before they happen, not after. | Must |
| 5.6 | As a new parent, I get feeding guidance by age and weight — what, how much, how often, water. | Must |
| 5.7 | As a new parent, I get training-window guidance: when to start toilet/litter training, socialisation window, recall. | Should |
| 5.8 | As a new parent, I can mark milestones done and the timeline reflects it. | Should |
| 5.9 | As a new parent, I get a weekly "what's coming" notification. | Should |

**Acceptance (5.4):** Given a UK puppy with estimated DOB 8 weeks ago, the expected schedule shows
the primary course items for dogs/UK with expected dates; confirming one creates the vaccination
record in E2 and removes it from *expected*. Region is required before this screen renders.

### E6 — Ask the app (assistant)

| # | Story | Pri |
|---|---|---|
| 6.1 | As a new parent, I can ask a question in plain language and get an answer grounded in the reviewed journey content for my pet's species and age. | Should (v1.1) |
| 6.2 | As a parent, if I describe symptoms, the assistant does not diagnose; it tells me what warrants a vet today vs. can wait, and gives me my vet's number. | Must, if 6.1 ships |
| 6.3 | As a parent, every answer shows what content it drew on, and "I'm not sure — ask your vet" is a valid answer. | Must, if 6.1 ships |
| 6.4 | As the product, questions the content couldn't answer are logged (anonymised) so authors know what to write next. | Should |

**Acceptance (6.2):** Given "my puppy has been vomiting since this morning and won't drink", the
response contains no diagnosis or medication suggestion, flags it as *see a vet today*, and
surfaces the stored vet and out-of-hours numbers.

### E7 — Wondering: the virtual pet

| # | Story | Pri |
|---|---|---|
| 7.1 | As a prospective parent, I choose a species and create an avatar with a name and look. | Should (v2) |
| 7.2 | As a prospective parent, I choose the pace — a year in 30 days, or 90 — and the journey runs from Day 1. | Should (v2) |
| 7.3 | As a prospective parent, I get notifications from the pet's day: "6:10 a.m. — Biscuit needs to go out", "Chewed a shoe. This is normal at 4 months", "Second vaccination: £45". | Should (v2) |
| 7.4 | As a prospective parent, I get small interactions: feed, walk, train, and see the effect. | Could |
| 7.5 | As a prospective parent, at the end I get an honest summary: hours a week, money spent, holidays affected, and the good bits. | Should (v2) |
| 7.6 | As a prospective parent who decides yes, my avatar's species and what I learned carry into the real journey when I add a real pet. | Could |

**Acceptance (7.3):** Given a virtual dog at accelerated day 60 (≈ real puppy at 4 months), the
notifications for that day are drawn from the same E4 content as the E5 guide would show a real
4-month-old dog. No content exists only for the virtual pet.

### E8 — Sharing with a provider (the bridge to Phase 2)

| # | Story | Pri |
|---|---|---|
| 8.1 | As a parent, I can generate a **share link or PDF** for a pet — a "boarding card" — containing exactly the fields a hostel or sitter needs: vaccinations, parasite prevention, allergies, food, medications, temperament, vet and emergency contacts. | Must |
| 8.2 | As a parent, I choose which sections the link includes and how long it lives. | Must |
| 8.3 | As a parent, I can revoke a link, and it stops working immediately. | Must |
| 8.4 | As a parent, I can see when a link was opened. | Should |
| 8.5 | When providers join the platform (Phase 2), a share becomes a consent grant to that provider, with the same scoping — no new model. | Design constraint |

**Acceptance (8.1):** Given a share configured *without* medical history, when opened, the page
shows vaccinations, parasite prevention, allergies, food, temperament and contacts, and no medical
history is present in the response body — not merely hidden.

**Why this is in v1:** it delivers the "hostel sees the pet's information" goal *today*, with no
provider on the platform, and it is directly usable with the boarding-facility contact.

### E9 — Cross-cutting

| # | Requirement | Pri |
|---|---|---|
| 9.1 | Push notifications on the chosen platform (see open question 1). | Must |
| 9.2 | Privacy: consent-based sharing, export, deletion; no health inference stored that the parent didn't enter. | Must |
| 9.3 | Region on the account, driving vaccination schedules and content selection. | Must |
| 9.4 | Works well on a phone; the primary device is a phone. | Must |
| 9.5 | Accessibility to WCAG 2.2 AA. | Should |
| 9.6 | Observability, audit of consequential actions, isolation between parents' data. | Must |

---

## 5. What v1 is, and what it is not

**v1 — "Living + Beginning", dogs and cats, one region**

E1, E2, E3, E4, E5, E8, E9. A parent can keep a complete record, get reminders, follow the
Day 1 → Year 1 journey, and hand a hostel a boarding card. Every piece delivers value alone.

**v1.1 — "Ask the app"**

E6, once there is enough reviewed content to ground it. Building it before the content exists
produces a chatbot that makes things up.

**v2 — "Wondering"**

E7, once E4's content covers the full year for both species and the E5 experience has been used by
real new parents. The virtual pet is the most delightful part of the vision and the most dependent
on everything under it being right.

**Phase 2 — providers**

Hostels, sitters, walkers, vets, shops. The CRM work already specified in `.claude/`. The bridge is E8.

**Not in any of these:** marketplace/e-commerce, third-party product catalogues, social features,
multi-species beyond dogs and cats, breed-specific content.

---

## 6. Assumptions (correct these)

1. Dogs and cats only, in v1 and v2.
2. One region for content in v1 — to be named (open question 2).
3. Journey content is authored and reviewed by a person with veterinary competence before
   publication. The app does not generate care guidance from a model without review.
4. The assistant, when built, is retrieval-grounded on reviewed content, not free-form.
5. The virtual pet is notification-driven and lightweight, not a real-time game.
6. The primary device is a phone; the app must notify reliably.
7. The existing stack (Next.js, Postgres, Prisma, self-hosted auth) is kept; the frontend delivery
   mechanism is the open question.

---

## 7. Open questions — these block refinement

1. **Platform.** "Download the app" and daily notifications point to mobile. The current stack is a
   Next.js web app. PWA (one codebase, push works but is second-class on iOS) or a native app via
   React Native/Expo (a real app, more work, new skills needed in `.claude/`)?
2. **Region for content.** Vaccination schedules differ materially by country (rabies is routine in
   India and not in the UK; leptospirosis is core in the UK). E5.4 cannot be written without this.
3. **Content source.** Who writes and reviews the journey content? This is the largest single
   effort in v1 and it is not code.
4. **The virtual pet's honesty.** "The loving and bright side" as originally described, or the
   whole year honestly (principle 3)? Recommendation: honest.
