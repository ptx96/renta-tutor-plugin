# Modelo 151: Filing and Income Reference

Research checked: 2026-10-03. Mapping baseline: the 2023-onward form approved by
Orden HFP/1338/2023. Rates below distinguish 2023-2024 from 2025. Before another
exercise or a live entry, reverify law, instructions, form and calendar. Do not
assume a filing year, deadline, or field number from the prior return.

## Official Source Register

| ID | Source and locator | Applicability |
| --- | --- | --- |
| M1 | [Modelo 151 procedure](https://sede.agenciatributaria.gob.es/Sede/procedimientoini/G615.shtml), exercise-specific presentation links | Separate services for 2023 onward and through 2022 |
| M2 | [Completion instructions](https://sede.agenciatributaria.gob.es/Sede/todas-gestiones/impuestos-tasas/impuesto-sobre-renta-personas-fisicas/modelo-151-decla_____los-trabajadores-desplazados-espanol_/instrucciones-cumplimentacion-ejercicio-2023-siguientes.html), regime content, pages A1-G and Hoja Informativa | Form for 2023 onward; explicit 2023-2024 and 2025-onward scales on current page |
| M3 | [Technical form help](https://sede.agenciatributaria.gob.es/Sede/ayuda/consultas-informaticas/presentacion-declaraciones-ayuda-tecnica/modelo-151.html), entry, validation and presentation | Current UI documentation, not proof of a live session |
| M4 | [Orden HFP/1338/2023](https://www.boe.es/buscar/doc.php?id=BOE-A-2023-25416), Modelo 151 and annex | New form from exercise 2023 |
| M5 | [LIRPF article 93](https://www.boe.es/buscar/act.php?id=BOE-A-2006-20764#a93), calculation specialties | Historical version applicable to tax year |
| M6 | [TRLIRNR](https://www.boe.es/buscar/act.php?id=BOE-A-2004-4527#a13), articles 13, 24-26 | Source, base and deductions as modified by article 93 |

These URLs were inspected during implementation; record a fresh retrieval date
and applicable version in each case. If a source is unreachable or conflicts,
leave the dependent rule/mapping REQUIRES_VERIFICATION.

## Filing Context

Taxpayers to whom the special regime applies file Modelo 151 (M2/M4/M5), not
Modelo 100. Do not import ordinary IRPF non-filing income thresholds. Establish
exercise, ongoing regime and period before treating the form as applicable.

The official procedure has a dedicated 151 service. For 2023 onward the current
presentation selector targets `/wlpl/OVME-COMN/151/E2023/index.zul`; `E2023` is
the service-version path, not evidence that the selected return year is 2023.
Use M1's current link, then inspect the actual exercise. The older service is
separate and its mappings are not provided by this plugin.

AEAT requires NIF and prior census identification. Technical help describes
Cl@ve Movil and certificate/DNIe access (M2/M3). The user authenticates directly.
Do not substitute ordinary Renta reference-number access without verification.

M2 links the general filing period to the year's IRPF filing order and mentions
a direct-debit endpoint. M3 tells users to consult the calendar for each period.
Verify the actual Modelo 151 tax-year calendar and payment deadlines; do not
carry a generic June date or Modelo 100 payment options into the user's case.
Late, corrective/complementary, or prior-year returns require specific review.

## Classification and Tax Treatment

| Concept | Verification and boundary | Form destination once verified | Source |
| --- | --- | --- | --- |
| Employment, including foreign payroll | Verify activity dates, gross cash vs in-kind, payer and actual withholding; article 93 deems employment during regime application Spanish-source with temporal exceptions | A1; type 01 for ordinary employment, other verified type if applicable | M2 A1 and Hoja Informativa; M5 |
| Benefits in kind | Distinguish valuation, income on account and amounts passed to employee; check the specific employment-in-kind exemption permitted by the regime | A1 nature E; taxable gross uses valuation + ingreso a cuenta - repercutido | M2 regime content and A1; M5 |
| Interest | Establish legally relevant source, underlying product, gross/net and withholding; bank location alone is insufficient | A2, type 05 | M2 A2 and Hoja Informativa; M6 article 13 |
| Dividends | Establish issuer/source, gross receipt and actual tax; broker custody country does not decide source | A2, type 04 | M2 A2 and Hoja Informativa; M6 article 13 |
| Capital gains/losses | Verify underlying assets, source, dates, acquisition/disposal evidence and transaction type; no automatic netting of losses | C for specified gains subject to withholding, D for property, E2 for other disposal gains | M2 C/D/E2; M5 |
| Spanish property rent/imputation | Obtain use, ownership, dates and relevant property evidence; ordinary rental deductions are not automatic | A1 with verified property-specific type; D for disposal | M2 A1/D and Hoja Informativa; M6 |
| Economic activity | Entry regime may allow specified activities; it is not categorically incompatible. Detect and refer, do not implement an autonomo workflow | B exists, but its workflow is unsupported in this MVP | M2 B; M5 |
| Other receipts | Identify nature and Spanish-source rule before a destination; no catch-all inclusion based on a bank transfer | Appropriate verified epigraph, otherwise UNKNOWN | M2/M5/M6 |

Keep foreign provenance as an attribute and classify underlying nature. An item
must not be duplicated as both EMPLOYMENT and FOREIGN_INCOME. Keep excluded
receipts/losses and their reasons in the review rather than deleting evidence.

For employment use gross taxable income, not net bank pay. Do not subtract
employee Social Security, ordinary IRPF employment expenses/reductions or
personal/family minima automatically. Record actual withholding separately;
never infer it solely from the theoretical 24% rate. General ordinary
deductions and Autonomous Community catalogs are outside scope (M2/M5).

The special regime does not allow compensation between the accumulated
in-scope rents as in ordinary IRPF. M2 explicitly prohibits negative amounts in
the positive-income/gain fields and netting gains against losses. Unsupported
asset/source/cost-basis or loss questions require review, not an investment
calculation engine. Do not copy Modelo 100 investment/crypto casillas.

## Limited Arithmetic Checks

M2/M5 provide the general scale: 24% up to EUR 600,000; 47% on excess. It
applies to the general base, not only salary. The special ahorro scale applies
to the article 25.1.f TRLIRNR category; do not use a flat 19% on all savings.

| Ahorro slice in EUR | 2023-2024 | 2025 |
| --- | --- | --- |
| 0-6,000 | 19% | 19% |
| 6,000-50,000 | 21% | 21% |
| 50,000-200,000 | 23% | 23% |
| 200,000-300,000 | 27% | 27% |
| Above 300,000 | 28% | 30% |

These are marginal slices, not rates on the whole amount (M2/M5). M2 currently
describes the latter scale as "2025 y siguientes"; do not guarantee later-year
applicability without rereading current law. Monetary entries use EUR and two
decimals. Use decimal arithmetic, explicit rounding and form reconciliation for
checks, not a standalone tax engine.

## Deductions and Foreign Tax

M2/M5 recognize qualifying donations and actual withholding/payments on account,
plus the specific foreign-tax credit for employment and qualifying
entrepreneurial activity. Verify the selected year's donation conditions,
certificate and limits before mapping G [24]; no blanket "no deductions" rule.

The credit is not simply all foreign tax. Article 93/M2 refer to article 80 and
a 30% cap on the relevant quota for the specified employment/entrepreneurial
income. Verify eligible income, actual foreign tax, effective-rate calculation,
caps and year before mapping G [27]. Foreign investment tax is not automatically
Spanish withholding or a credit. Complex credit calculations are review items,
not implemented calculations in this MVP.

Do not silently repair ambiguous official text. In the inspected instructions,
the G [27] discussion cross-references B [12]/[13] inconsistently and examples
contain arithmetic/label inconsistencies. Cross-check applicable law, the
approved form and live form or obtain official advice before using those parts.
The simple employee/Spanish-interest mappings do not rely on those paragraphs.

## Final Checks

Reconcile the verified items with the section-qualified field reference,
actual withholding and the form's calculated G result. AEAT's validation checks
form constraints, not the completeness of source documents or every tax rule.
Do not mark readiness with unresolved treatment, evidence, mappings, deadlines,
or warnings. Nothing is signed, submitted, or paid by this plugin.