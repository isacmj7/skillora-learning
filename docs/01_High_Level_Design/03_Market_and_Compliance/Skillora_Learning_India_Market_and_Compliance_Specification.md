---
name: Skillora Learning — India Market, Localisation & Compliance Specification
version: 1.0
purpose: Makes Skillora competitive, locally relevant and legally compliant for India (web + mobile): positioning, learner segments, localisation, payments, regulation, channels, pricing hypotheses, trust
applies_to: All Skillora specifications (Master Spec, SaaS & Monetization, Admin, Credentials, Resume Studio, Web v2, Mobile v2)
status: Product requirements + compliance checklist. Not legal advice — have counsel validate before launch.
---

# Skillora Learning — India Market, Localisation & Compliance Specification

## 1. WHY THIS DOCUMENT EXISTS

The core specs describe a globally neutral product. India has specific realities that decide whether a learning product wins or fails: Android-first and price-sensitive users, UPI-first payments, GST and RBI rules, the Digital Personal Data Protection regime, strict consumer-protection rules for edtech/coaching advertising, campus-driven distribution, and learners who need job outcomes (placements, service-company and product-company interviews).

Rule: **every India requirement here is configuration or content where possible** (Admin Portal), so the same platform can later serve other markets.

## 2. MARKET POSITIONING

### 2.1 Landscape (as reported by third-party sources in 2026; verify before using in marketing)

| Segment | Examples | Typical price point reported | What they do well | Gap Skillora can exploit |
|---|---|---|---|---|

Sources: [Coding Ninjas fees](https://www.codingninjas.com/blog/faqs/coding-ninjas-fees-course-roi/), [Scaler vs NxtWave vs others](https://www.collegesimplified.in/post/scaler-vs-newton-vs-polaris-vs-nxtwave-school-of-technology-review-2026-register-with-cs500-code-ful), [NxtWave fees](https://www.ccbp.in/blog/articles/nxtwave-academy-program-fees-emi-scholarships), [mock interview platforms in India](https://usegreenroom.app/blog/best-mock-interview-websites-india). Prices change often; re-check quarterly.

### 2.2 Recommended positioning (hypothesis to validate)

> **Skillora is the affordable AI career coach for Indian software engineers: it learns who you are, teaches you at your level, proves it with verified credentials, writes your job-ready resume, and rehearses the exact interview you are about to face.**

Differentiators to protect (all already in the specs): personalised adaptive route per learner; level-adaptive visual teaching; Python + SQL (PostgreSQL & MySQL) in-browser practice without autocomplete; JD → gap → route → mock interview loop; evidence-based badges/certificates; truthful AI resume; one platform on web and mobile.

What Skillora should **not** try to be at launch: a full-time bootcamp, a degree provider, a generic video marketplace.

### 2.3 Learner segments (drive content, pricing and channels)

| Segment | Needs | Product emphasis |
|---|---|---|
| **College students / freshers (largest)** | Fundamentals, DSA, aptitude, campus-placement rounds (service + product), first resume, HR/behavioural | Free/low-cost tier, placement-prep tracks, Hinglish/vernacular help, college/TPO channel |
| **Job seekers 1–5 yrs** | Skill upgrade, interview rounds at product companies and GCCs, JD-targeted prep | Interview Master, JD gap, system design intro, resume tailoring |
| **Career switchers / non-CS graduates** | Foundations, a credible portfolio, guidance | Beginner levels, projects, credentials, gap route |
| **Working engineers / seniors** | System design, GenAI, cloud, leadership interviews | Senior levels, deep dives, mock interviews with senior rubric |
| **Colleges / training partners / employers (B2B)** | Cohort management, analytics, white-label | Tenants, seats, reports (SaaS phases S7+) |

### 2.4 India-specific interview content (admin-managed question banks and blueprints)

- Campus placement: aptitude (quant, logical, verbal), coding rounds with time pressure, technical + HR interview, group-discussion basics, company-style blueprints (service companies and product companies).
- Technical: Python/Django/Flask/FastAPI, SQL (PostgreSQL & MySQL), DSA, system design, cloud/DevOps, GenAI — as already specced.
- HR/behavioural in the Indian context: "tell me about yourself", notice period, CTC expectations and negotiation, relocation, bond/agreement questions, career-gap handling.
- Resume formats suited to Indian ATS and recruiters (no photo/age/marital status by default; configurable), fresher vs experienced templates.
- Indian-context examples in lessons (UPI, IRCTC-like booking, e-commerce, fintech, GST invoicing) to improve relatability and recall.

## 3. LOCALISATION & LANGUAGE

| Area | Requirement |
|---|---|
| **UI languages** | English first; architecture ready for Hindi and major Indic languages (Tamil, Telugu, Kannada, Malayalam, Marathi, Bengali, Gujarati…) via admin-managed translation tables; priority order set by demand data |
| **Content languages** | Lessons/visuals/translations managed per language in Admin (translation workflow, fallback to English); code, identifiers and technical terms stay in English with optional glossary tooltips |
| **Hinglish tutor mode** | AI tutor and explanations can mix Hindi and English on request; learner-selectable language style |
| **Voice** | Indian-English accents and Hinglish code-mixing are first-class test cases for STT/TTS provider selection; evaluate word-error rate on Indian speakers before choosing a provider; selectable Indian voices; text fallback always available |
| **Fonts** | Fonts covering Indic scripts; test rendering on low-end Android |
| **Accessibility** | Screen-reader support for supported languages; captions/transcripts for video and voice |

## 4. DEVICE, NETWORK & PERFORMANCE REALITY

Context (2026 third-party data): Android is by far the dominant mobile OS in India (reported above 90%), the low/mid-range device segment is large, and mobile data is cheap but connectivity quality varies. See [StatCounter India OS share](https://gs.statcounter.com/os-market-share/mobile/india) and [IDC India smartphones](https://www.idc.com/resource-center/blog/india-smartphone-market-q1-2026/).

Requirements (budgets in the Engineering spec §3):

- **Small install size and lean runtime:** lazy-load heavy modules (code editor, whiteboard, animations); asset compression; split APKs/AAB by ABI/density.
- **Data-lite mode:** user-selectable low-data setting (static SVG visuals instead of animations, no autoplay, compressed media, deferred downloads on Wi-Fi only, offline packs).
- **Offline-first learning:** downloadable lesson packs (text + pre-rendered diagrams + code examples), local progress queue, resilient sync.
- **PWA / lightweight web:** installable, cached shell for browsers on low-end phones and shared college lab PCs.
- **Network resilience:** flaky-network handling for voice interviews (reconnect, resume, text fallback, local answer buffering).
- **Shared devices and cyber-café/lab usage:** quick logout, no persistent sensitive data on shared browsers, session timeout options.

## 5. AUTHENTICATION & COMMUNICATION CHANNELS

- **Mobile-number OTP login is primary** (India norm), plus Google sign-in and email/password; account linking and recovery. SMS OTP requires compliance with TRAI DLT registration (sender ID and templates) — plan lead time.
- **WhatsApp Business** (opt-in) for reminders, interview-date countdowns, course updates, receipts; **SMS** for OTP and critical alerts; **push** and **email** as secondary. Per-channel consent and quiet hours; template approval handled via provider (admin-managed templates).
- Referral and invite flows work over WhatsApp share links.
- Language preference drives message language.

## 6. PAYMENTS (INDIA-FIRST)

> **Release 1.0 scope (2026-10-22):** only one-time payments through one gateway's hosted checkout (UPI, cards, net banking, wallets), GST-inclusive prices, GST invoices and a stated refund policy are required in 1.0 (Billing Lite). E-mandate, UPI AutoPay, EMI built by Skillora and recurring billing in this section apply when subscriptions are added. See `../../05_Release_1_0/01_Scope/Skillora_Learning_Release_1_0_Billing_Lite_and_Trial_Specification.md` §3 and §5.

### 6.1 Methods and providers

- **UPI first** (intent/collect/QR), then cards (debit/credit), net banking, wallets, **EMI / no-cost EMI / BNPL** for higher-priced programs, and pay-later options where compliant.
- Payment success-rate monitoring per method and issuer bank; smart retry and fallback methods; abandoned-checkout recovery.
- Settlement reconciliation, GST reports, refunds to original instrument, payout timelines.

### 6.2 Recurring payments (RBI e-mandate framework)


Product implications:
- Billing adapter must support mandate registration, pre-debit notifications, retries within network rules, failure handling, and mandate cancellation from the billing portal.
- **Annual and one-time plans** are the natural India default (lower mandate failure and churn); offer monthly via AutoPay as an option.
- Clear, easy cancellation (also a dark-pattern requirement).

### 6.3 GST and invoicing

- Online courses/coaching from a private provider are generally taxed at **18% GST (SAC 9992)** unless an education exemption applies; tax invoices must carry GSTIN and SAC; GST liability can arise on **advance receipts**, not only on delivery (so annual fees are taxable when received). Foreign providers selling to Indian consumers fall under OIDAR rules. See [GST on online education](https://busy.in/gst-rates/online-education/) and [GST for OIDAR](https://www.patronaccounting.com/gst-returns-for-oidar). Confirm treatment (including any exemption for certain credentialed programs) with a CA.
- Billing must support: GSTIN capture for B2B buyers, place-of-supply logic (CGST/SGST vs IGST), compliant tax invoices and credit notes, invoice numbering series, e-invoicing thresholds if applicable, TCS/TDS handling where relevant, GSTR-ready exports.

### 6.4 Pricing hypotheses (to test, not to assume)


Hypothesis for Skillora (validate with experiments after launch usage data):
- **Free:** intro courses, basic practice, limited tutor, resume preview.
- **Pro (individual):** a price clearly below premium mentored programs and competitive with the AI-mock tools, with annual discount; **student plan** with verification (college email/ID) at a lower price; **campus/cohort pricing** for colleges.
- **Course/track purchases:** low-ticket course prices plus bundles; **EMI** for higher-priced tracks/programs.
- **Credit packs** for mock interviews/voice minutes (pay-as-you-go suits students).
- Regional/PPP and festival/exam-season promotions; scholarships/need-based discounts.

Unit-economics guardrail: price and quotas must keep **AI + sandbox + voice cost per active paying user** comfortably below ARPU (Engineering spec §9).

## 7. COMPLIANCE CHECKLIST

### 7.1 Data protection — Digital Personal Data Protection Act, 2023 and DPDP Rules, 2025

Reported timeline: Rules notified 13 Nov 2025; consent-manager provisions phased at 12 months; **main Data Fiduciary obligations apply by about 13 May 2027** (18 months). Children's data (under 18) needs **verifiable parental consent**; tracking, behavioural monitoring and targeted advertising directed at children are prohibited. **Skillora Learning avoids this whole area by being an adult-only service (18+; Higher Secondary completed), per `../../05_Release_1_0/01_Scope/Skillora_Learning_Release_1_0_Audience_and_Eligibility_Policy.md` (decision of 2026-10-22).** See [EY guide to DPDP Rules 2025](https://www.ey.com/en_in/insights/cybersecurity/transforming-data-privacy-digital-personal-data-protection-rules-2025) and [PIB notification](https://static.pib.gov.in/WriteReadData/specificdocs/documents/2025/nov/doc20251117695301.pdf). The earlier assumption that many users would be minors no longer applies: the product does not register anyone under 18, so no guardian-consent flow is built; the adult-only gate, detection and deletion steps in the Audience and Eligibility Policy are the design constraint instead.

Design requirements:
- **Consent:** separate, clear, itemised notices in plain language (and supported Indian languages) stating purpose; granular consents (e.g. analytics, marketing, voice recording, resume processing, AI processing, public profile); consent records stored with version/time; easy withdrawal as simple as giving.
- **Age and eligibility gate (adult-only):** collect date of birth and highest education at signup; **under-18 attempts are declined politely with no account and no data kept; Higher Secondary or equivalent must be completed**; keep only birth year, an 18+ flag and education level afterwards; signed eligibility statement at signup and checkout; underage detection, 24-hour suspension, 7-day deletion and full refund of unused period. No guardian-consent flow, no minors' restrictions. Details: Audience and Eligibility Policy.
- **Data principal rights:** access, correction, erasure, grievance redressal, nomination — self-service in the app; response SLAs; **grievance officer** contact surfaced; data export (also supports portability expectations).
- **Purpose limitation & minimisation:** collect only needed data; resume, voice and interview transcripts classified **sensitive-by-policy** with short, configurable retention; deletion on request and on inactivity policy.
- **Security safeguards & breach handling:** encryption, access control, logging; breach notification process to the Data Protection Board and affected users per Rules; vendor/processor agreements (LLM, STT/TTS, analytics, email/WhatsApp).
- **Cross-border transfers:** know which providers process data outside India; keep a transfer register; prefer India-region processing where possible; disclose in notice.
- **No training on learner data** by default; AI processing disclosed; opt-in for any quality-improvement use.
- **Significant Data Fiduciary** readiness (audits, DPO, impact assessments) if thresholds are met later.
- Admin Portal: consent text/versions, retention policies, DSAR (data subject request) queue, breach log, processor registry, notification templates.

### 7.2 Consumer protection (CCPA) — dark patterns and coaching advertising

- **Dark patterns (2023 guidelines, 13 patterns incl. basket sneaking, confirm-shaming, forced action, interface interference, trick questions)** have been enforced against edtech (e.g. forcing personal info before accessing "free" courses, pre-added donations). Skillora must: never pre-select paid add-ons or donations; no confirm-shaming copy; no forced sign-up to see free content previews where not necessary; no false urgency/fake countdowns/fake scarcity; clear cancel flows; transparent total price. See [CCPA dark patterns enforcement](https://www.pib.gov.in/PressReleasePage.aspx?PRID=2268302&reg=3&lang=1).
- **Misleading advertising in coaching (guidelines issued 13 Nov 2024):** no unverified success-rate claims, no guaranteed placements/ranks/selections; ads must accurately state duration, faculty qualifications and fees. See [CCPA coaching guidelines summary](https://corporate.cyrilamarchandblogs.com/2024/11/ccpa-schools-coaching-centres-on-misleading-advertisements/).
  - Skillora rules: **no placement/job guarantees** anywhere (already a Resume Studio rule); any outcome statistic must be verified, dated and footnoted; testimonials only from verified learners with consent; certificate disclaimers (not a degree/accreditation unless real); fees, duration, schedule and refund terms shown before purchase.
- **Consumer Protection (E-Commerce) rules:** grievance officer, clear seller details, refund/cancellation policy, display of total price incl. taxes.
- Admin Portal: claim/ad-copy approval workflow with compliance review, testimonial consent records, mandatory refund-policy fields on every paid offering.

### 7.2A AI in hiring — pointer only (Skillora Screen is a separate future product)

Recruiter-side screening is **not part of Skillora Learning**; its legal and responsible-AI framework lives in `02_Skillora_Screen/01_High_Level_Design/Skillora_Screen_Responsible_AI_Security_and_Compliance.md`. Summary for context: India has no dedicated AI-hiring statute yet, but DPDP consent/notice/rights, anti-discrimination, disability-rights and labour-law principles apply; multinational and GCC customers will also expect EU/US-style safeguards (EU AI Act treats recruitment AI as high-risk — the reported timeline was moved to 2 December 2027 with a ban on emotion inference of candidates already in force; NYC Local Law 144 requires bias audits and candidate notice). Product rules (full detail in `Skillora_Screen_Product_Specification.md` §8–9): advisory scores only with human decision; candidate notice and explicit, purpose-specific consent; Skillora as processor under a DPA for employer screening data; no emotion/personality inference; fairness monitoring and accommodations; short retention; hiring question banks isolated from practice banks. See [EU AI Act and hiring](https://iqmetrics.org/news/eu-ai-act-hiring-assessments-deadline-2027/) and [AI recruitment legality in India](https://www.thehirehub.ai/guides/ai-recruitment-india). Confirm current law with counsel.

### 7.3 Other regulatory items to confirm with counsel

| Item | Note |
|---|---|
| **CERT-In directions (2022)** | Incident reporting within a short window (commonly cited: 6 hours) and log retention within India (commonly cited: 180 days) for covered entities — confirm applicability and current text |
| **IT Act / intermediary rules** | If community/UGC features exist: moderation, takedown, grievance officer |
| **RBI payment aggregator rules** | Use licensed gateways; do not store card data (PCI scope minimised via hosted checkout) |
| **TRAI DLT** | SMS sender and template registration |
| **WhatsApp Business policy** | Opt-in and template approval |
| **App store policies (Apple/Google)** | Billing rules for digital goods, children's data policies, data-safety disclosures |
| **Copyright & content licensing** | Licensing of any imported books/courses; AI-generated content disclosure; third-party interview-question sources |
| **Trademark / domain** | Clear "Skillora" mark and skilloralearning.com; avoid confusingly similar names |
| **Accessibility** | RPwD Act expectations and WCAG-aligned design |
| **Company/tax setup** | GST registration, invoicing, TDS/TCS, FEMA for foreign receipts if relevant |

## 8. TRUST & SAFETY

- Verified-learner testimonials; transparent credential verification (Credentials spec §6).
- **AI transparency:** label AI-generated content/feedback; explain scoring; allow learners to appeal an interview score or credential decision and request human review (add-on service later).
- **Bias monitoring:** evaluate interview scoring across accents, language fluency, gender-coded phrasing and college tier; fairness test sets in the AI evaluation suite; score communication separately from technical correctness so accent or fluency never silently lowers technical scores.
- Content moderation for any community/UGC; abuse reporting; standard adult-platform moderation (no minors in the audience).
- Honest marketing and no income/placement claims.

## 9. DISTRIBUTION & GROWTH (INDIA)

| Channel | Mechanism |
|---|---|
| **Colleges / TPO / training partners** | B2B2C tenants, cohort dashboards, bulk seats, campus-placement-season bundles, faculty tools; revenue share or campus licences |
| **Campus ambassadors & referrals** | Referral credits, ambassador tracking, WhatsApp share |
| **Content-led SEO** | Public free lessons, interview question pages, roadmap pages, vernacular content |
| **YouTube / Instagram / LinkedIn / Telegram / WhatsApp communities** | Short-form teaching, free webinars, community groups |
| **Partnerships** | Hiring partners, NSDC/NASSCOM-style skilling ecosystems and state skilling programmes where eligible (explore, don't assume) |
| **Seasonality** | Campus hiring seasons, results seasons, appraisal/switch seasons — schedule campaigns and capacity accordingly |

Admin supports campaign codes, UTM capture, partner dashboards and per-campus analytics.

**Employer tie-ups:** companies can enrol employees in project/role-based micro-courses (B2B upskilling). Strategy and competitive positioning: `Skillora_Learning_Competitive_Landscape_and_Enterprise_Strategy.md`; design: `Skillora_Learning_Enterprise_Upskilling_Specification.md`. Skillora acts as a **data processor** for company customers under DPDP (DPA, sub-processor list, employee notice).

## 10. COMMUNITY & SUPPORT EXPECTATIONS

Indian learners expect **fast doubt resolution**. Provide layered support:

1. AI tutor (instant, in-context, Hinglish capable);
2. searchable knowledge base / FAQs;
3. community Q&A (moderated) per course/module (later phase);
4. mentor / live doubt sessions as a paid add-on or cohort feature;
5. human support desk with SLAs (email/WhatsApp/in-app chat), ticketing integrated with the Admin Portal (learner context, consent-based access).

## 11. PRODUCT REQUIREMENTS SUMMARY (INDIA)

- [ ] Mobile OTP login; WhatsApp/SMS/push/email channels with consent and DLT/template compliance.
- [ ] Adult-only eligibility gate (18+ and Higher Secondary completed) with signed statement, underage detection and deletion runbook; no marketing aimed at under-18s.
- [ ] DPDP consent, rights, retention, breach and processor-registry tooling in Admin.
- [ ] UPI-first checkout, EMI, e-mandate-compliant recurring billing with 24-hour pre-debit notice, GST invoices (SAC 9992), tax-inclusive pricing.
- [ ] Dark-pattern-free checkout and marketing copy review workflow; no placement guarantees.
- [ ] Android-first performance, data-lite mode, offline packs, PWA.
- [ ] Hindi/Hinglish and Indic-language readiness; Indian-accent voice evaluation.
- [ ] Campus/B2B2C tenant features and campaign tracking.
- [ ] India-specific interview content (campus rounds, HR/CTC/notice-period) and resume templates.
- [ ] Multi-layer doubt support.
- [ ] AI transparency, appeal path and bias monitoring.
- [ ] India-region hosting (primary + DR in-country), processor transfer register.
