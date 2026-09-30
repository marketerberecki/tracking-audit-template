# Tracking Audit Framework

Use this reference after the initial scope and evidence review. Apply only the layers relevant to the client's stack and record unavailable access as a limitation.

## 1. Scope and intake

Collect:

- business model and primary outcomes: purchase, qualified lead, registration, subscription, offline conversion, or another client-defined result;
- critical funnel steps and success conditions;
- domains, subdomains, payment providers, checkout providers, iframe flows, and cross-domain paths;
- platforms in use: GA4, Google Tag Manager, Google Ads, Meta, TikTok, CMP, server-side GTM, CRM, ecommerce backend;
- reporting source of truth, time zone, currency, order status, and refund treatment;
- platform access and authority to inspect or change each system.

Create an evidence register with source, date, scope, and confidence. Browser-side observations are useful but cannot prove GTM configuration, backend behavior, server-side processing, or account settings.

## 2. Architecture and deployment inventory

Map the data route for each critical outcome:

`user action -> site/app -> DataLayer or application event -> web tag -> optional server endpoint -> destination platform`

Inventory every direct snippet, CMS module, plugin, GTM tag, native platform integration, server-side container, and custom script. Flag parallel or duplicate paths; retired accounts, pixels, Universal Analytics tags, test tags, and inactive containers; unclear ownership; unversioned production changes; and tag firing that bypasses the documented consent plan.

## 3. Web and DataLayer

Check that event names, trigger conditions, parameter types, and timing match the agreed taxonomy. For ecommerce, verify relevant `items` data and transaction-level fields. For leads, distinguish successful submission or confirmed lead creation from a click or form attempt.

For purchase-like events, test a stable, unique `transaction_id` or equivalent; value, currency, quantity, discount, tax, and shipping handling as applicable; refresh, back navigation, thank-you-page re-entry, SPA navigation, and payment redirect behavior; and that failure and cancellation paths do not create a success conversion.

Do not send email address, phone number, name, or other direct personal data to GA4 or include it in URLs.

## 4. Google Tag Manager

Review tags, triggers, variables, folders, naming, notes, exceptions, consent checks, and active versions. Look for duplicate configuration tags, broad page-view triggers, multiple triggers for the same business outcome, and custom HTML with unclear ownership.

For each proposed change, identify the GTM version, reviewer, test evidence, publish decision, and rollback path.

## 5. GA4

Inspect the property, stream, direct versus GTM configuration, event and key-event definitions, internal/developer traffic, unwanted referrals, cross-domain settings, user ID, linked products, and legacy configuration.

Decide key-event status from the client's use of the event. Do not automatically make every funnel event a key event. Any changes to session timeout or engagement rules need a documented business rationale.

## 6. Advertising platforms

### Google Ads

Confirm primary versus secondary conversion roles, native conversion tags and imports, value/currency/ID handling, Conversion Linker, Enhanced Conversions, dynamic remarketing, and consent behavior. The same business outcome should not unintentionally create duplicate primary bidding signals.

### Meta and TikTok

Confirm that browser pixel, server events/API, plugins, and GTM do not double-send the same event. Verify standard event names, value/currency/content parameters, event IDs, browser-server deduplication, and platform diagnostics. Retain, change, or remove CAPI only after evaluating measurement quality, consent, data governance, operating cost, and maintenance capacity.

## 7. Consent, CMP, and data governance

Test at minimum default, accepted, rejected, and partial consent states, including returning users. Verify ordering of consent initialization and updates, `analytics_storage`, `ad_storage`, `ad_user_data`, and `ad_personalization` where applicable, cookie behavior, and network requests.

Describe consent logic technically, but refer legal basis, copy, regional logic, and UX approval to the client's legal/privacy owner. Avoid dark-pattern recommendations.

## 8. Optional server-side tracking

Audit server-side tracking only when it exists or is under consideration. Inspect endpoint/domain, access control, consent propagation, client and tag configuration, transformations, event ID deduplication, logging, monitoring, costs, data retention, and failure handling. Treat server-side deployment as an architecture decision, not a default upgrade.

## 9. Reconciliation and validation

Compare backend and analytics on aligned time zone, currency, time window, order state, refund handling, and transaction IDs. Suggested starting tolerance for transaction count and revenue is 5%, but agree the final threshold with the client. Platform attributed conversions do not need to match backend or GA4 exactly; explain attribution and modeling differences.

Test new and returning users, desktop and mobile, consent states, primary funnel, failures, refresh/back navigation, payment redirects, and browser/server deduplication when present.

Validate immediately after release, then again after 1-3 weeks of data collection.

## 10. Finding template

| Field | Content |
|---|---|
| ID and title | Short, unambiguous finding name |
| Scope | System, page/flow, platform, and environment |
| Evidence | Screenshot, request log, debug output, configuration view, or reconciled data |
| Observation | What happens and under which conditions |
| Severity | P0 critical, P1 high, P2 medium, or P3 low |
| Impact | Data integrity, campaign optimisation, privacy/compliance, or maintenance risk |
| Cause | Confirmed cause or explicitly labelled hypothesis |
| Remediation | Specific change, owner, dependency, and order of work |
| Acceptance test | Observable proof that the finding is resolved |
| Status | Open, in progress, testing, resolved, or accepted risk |

### Severity guide

- **P0:** Privacy breach risk, missing core conversion, or systematic duplicate purchase/lead measurement.
- **P1:** Material reporting or bidding distortion, broken consent behavior, or large unexplained backend discrepancy.
- **P2:** Incomplete funnel data, legacy configuration, operational fragility, or a non-critical event defect.
- **P3:** Documentation, organization, or low-impact reporting improvement.

## 11. Default delivery structure

1. Executive summary and decisions needed.
2. Scope, evidence, access, and limitations.
3. Current-state architecture.
4. Findings and priorities.
5. Client-specific event and conversion matrix.
6. Remediation roadmap, dependencies, estimates, and owners.
7. QA plan and acceptance criteria.
8. Post-implementation reconciliation and monitoring plan.
