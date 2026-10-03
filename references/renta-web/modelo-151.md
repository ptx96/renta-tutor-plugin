# Modelo 151: Field Mapping and Browser Procedure

Research checked: 2026-10-03. Baseline: the Modelo 151 form for 2023 onward.
Reverify the exercise-specific official instructions and live labels before
entry. The directory name follows the plan; the target is the **dedicated
Modelo 151 AEAT service**, not ordinary Modelo 100 Renta WEB.

## Mapping Sources

- **S1:** [AEAT 151 procedure](https://sede.agenciatributaria.gob.es/Sede/procedimientoini/G615.shtml),
  presentation links for 2023 onward vs through 2022.
- **S2:** [Official completion instructions](https://sede.agenciatributaria.gob.es/Sede/todas-gestiones/impuestos-tasas/impuesto-sobre-renta-personas-fisicas/modelo-151-decla_____los-trabajadores-desplazados-espanol_/instrucciones-cumplimentacion-ejercicio-2023-siguientes.html),
  the page/epigraph named in each row and Hoja Informativa for type codes.
- **S3:** [AEAT technical help](https://sede.agenciatributaria.gob.es/Sede/ayuda/consultas-informaticas/presentacion-declaraciones-ayuda-tecnica/modelo-151.html),
  tabs, records, G calculation, Guardar/Cargar, Vista previa, and Validar declaracion.
- **S4:** [Orden HFP/1338/2023](https://www.boe.es/buscar/doc.php?id=BOE-A-2023-25416),
  approved form and annex, applicable from exercise 2023.
- **S5:** [VS Code browser tools](https://code.visualstudio.com/docs/agents/run/browser-tools)
  and [tools reference](https://code.visualstudio.com/docs/agents/reference/tools-reference),
  built-in `browser` tool set and session sharing; host/version dependent.

Each mapping below is documented in S2 for the 2023-onward form, not verified
against an authenticated live taxpayer session. Keep rule_source, tax_year,
retrieved_at, live label and record ID in each case's mapping. If they disagree,
set UNVERIFIED_FIELD_MAPPING/BROWSER_STATE_MISMATCH; do not guess.

## Core Document-to-Field Map

Casilla numbers are **not globally unique**. Always include page, epigraph,
record vs total, and label. Values below are concepts, not user-specific values.
Taxpayer identifiers needed for form entry should be handled minimally.

| Concept | Page / section | Field / casilla and official meaning | Value/evidence required | Official locator |
| --- | --- | --- | --- | --- |
| Exercise | Page 1, principal | [1] Ejercicio | Verified income year; inspect actual selected exercise | S2, Pagina 1 |
| Taxpayer ID | Page 1, Contribuyente | [2] NIF; [3] Apellidos y Nombre | User-verified identity; not credentials | S2, Pagina 1 |
| Taxpayer role | Page 1, Condicion | [6] principal; [7] asociado | Documentary regime role, never assumed | S2, Pagina 1 |
| Residence region | Page 1 | [20] Comunidad o Ciudad Autonoma | Verified residence and official numeric code; not deduction eligibility | S2, Pagina 1 |
| Ordinary employment type | Page 2, A1 record | [01] Tipo de renta | Code 01 for employment except the code-11 activity; verify other types separately | S2, A1 + Hoja Informativa |
| Employment nature | Page 2, A1 record | [02] Naturaleza | D cash or E in kind; keep separate records by type/nature | S2, A1 |
| In-kind valuation | Page 2, A1 record | [03] Valoracion | Verified taxable valuation and exemption treatment | S2, A1 |
| In-kind payment on account | Page 2, A1 record | [04] Ingresos a cuenta | Actual payer payment on account | S2, A1 |
| Passed-on payment on account | Page 2, A1 record | [05] Ingresos a cuenta repercutidos | Actual amount borne by employee | S2, A1 |
| Gross taxable employment | Page 2, A1 record | [06] Rendimiento integro / Renta inmobiliaria imputada | Gross cash taxable income; for in kind [03]+[04]-[05]; not net pay | S2, A1 |
| Employment withholding | Page 2, A1 record | [07] Retencion o ingresos a cuenta | Actual verified withholding/payments, not foreign tax or Social Security | S2, A1 |
| Employer | Page 2, A1 record, Pagador | NIF, F/J, Apellidos y Nombre o Razon Social | Verified payer details; no invented numeric identifier | S2, A1, Pagador |
| Work obtained/taxed abroad | Page 2, A1 record, Datos adicionales | [08] Rendimiento integro obtenido y gravado en el extranjero | Verified qualifying portion already in [06], not additional income | S2, A1 |
| Work tax paid abroad | Page 2, A1 record, Datos adicionales | [09] Impuesto satisfecho en el extranjero | Actual eligible foreign tax evidence; separate from [07] | S2, A1 |
| General A1 income total | Page 2, A1 total | [15] Total rendimientos integros / Rentas inmobiliarias imputadas | Sum of A1 record [06] values, without duplicate documents | S2, A1 totals |
| A1 withholding total | Page 2, A1 total | [16] Total retenciones o ingresos a cuenta | Sum of A1 record [07] values | S2, A1 totals |
| Interest or dividend type | Page 4, A2 record | [01] Tipo de renta | 05 interest; 04 dividends, only after source/treatment verification | S2, A2 + Hoja Informativa |
| A2 nature | Page 4, A2 record | [02] Naturaleza | D cash or E in kind | S2, A2 |
| A2 gross income | Page 4, A2 record | [06] Rendimiento integro | Verified positive gross income; in-kind fields [03]/[04]/[05] follow documented formula | S2, A2 |
| A2 withholding | Page 4, A2 record | [07] Retencion o ingresos a cuenta | Actual verified applicable withholding, not automatically foreign tax | S2, A2 |
| A2 income total | Page 4, A2 total | [08] Total rendimientos integros | Sum of A2 record [06] | S2, A2 totals |
| A2 withholding total | Page 4, A2 total | [09] Total retenciones o ingresos a cuenta | Sum of A2 record [07] | S2, A2 totals |
| General base | Page 10, F | [17] Base liquidable general | A1 total [15] + B total [08] + E1 total [05] | S2, F |
| Ahorro base | Page 10, F | [18] Base liquidable del ahorro | A2 total [08] + C total [04] + D total [12] + E2 total [06] | S2, F |
| General quota | Page 10, G | [19] Cuota correspondiente a la base liquidable general | Form-calculated, compare only with verified tax-year scale | S2, G; S3 |
| Ahorro quota | Page 10, G | [20] Cuota correspondiente a la base liquidable del ahorro | Form-calculated, compare only with verified tax-year scale | S2, G; S3 |
| Total quota | Page 10, G | [21] Cuota integra total | [19]+[20], calculated comparison | S2, G; S3 |
| Donations deduction | Page 10, G | [24] Deduccion por donativos | Only specifically verified eligible deduction; not a general deductions slot | S2, G |
| Foreign-tax credit | Page 10, G | [27] Deduccion por doble imposicion internacional | Specific qualifying work/entrepreneurial credit after full rule/cap verification; complex cases need review | S2, G; article 93 LIRPF |
| Net quota | Page 10, G | [30] Cuota liquida total | max(0, [21]-[24]-[27]), calculated comparison | S2, G; S3 |
| Actual withholding total | Page 10, G | [33] Retenciones e ingresos a cuenta | A1 [16]+A2 [09]+B [09]+C [05]+D [13]+E1 [06] | S2, G |
| Prior IRNR quotas | Page 10, G | [34] Cuotas IRNR pagadas respecto de rentas incluidas | Only verified eligible same-year IRNR quotas; no duplicate credit | S2, G |
| Result | Page 10, G | [43] Resultado de la declaracion | [30]-[33]-[34]-[41]+[42]; [41]/[42] concern complementary returns, not presumed applicable | S2, G; S3 |

For the simple employee/interest case, use only A1/A2 and the corresponding
F/G totals. S3 says G fields calculate from other sections; compare/read them
rather than forcibly overwriting calculated controls. Verify which controls
the live form permits editing. Entries for unsupported B/C/D/E cases remain
blocked until their specific rule, value and record mapping are verified.

A1/A2 grouping uses the same type and nature, with descending positive amounts
according to S2. Preserve all payer/source detail and follow the actual form's
record capacity. Do not invent an overflow aggregation, delete existing
records, or replace the user's draft without separate verification/consent.

## Known Documentation Caveats

The inspected S2 page contains inconsistent legacy cross-references: page-1
phone/complementary-return paragraphs, B [12]/[13] in G [27], and a duplicated
misleading D [11] paragraph. Do not normalize these by guessing. The core
A1/A2 mappings above are explicitly attested by their own sections. For an
ambiguous affected field, consult S4, current instructions and live UI or
official assistance; keep it UNKNOWN until resolved. Never substitute Modelo
100 casillas such as 0027/0029/0588 merely because a secondary guide uses them.

## Safe Browser Workflow

1. Read S1 and use the current exercise-specific Modelo 151 link. Confirm the
   AEAT HTTPS domain and destination. Do not hard-code selectors or element IDs.
2. Use the built-in VS Code browser tools. Agent-opened tabs use isolated
   ephemeral sessions; a user-shared tab can retain their authenticated state
   (S5). Reuse an intentionally shared tab. Do not assume access to other tabs.
3. Let the user authenticate directly. Never request, read aloud, copy, store,
   or enter credentials, Cl@ve codes, certificate keys or session tokens. Resume
   after the user confirms authentication and sharing; do not bypass failures.
4. Inspect model, exercise, taxpayer, current tab/epigraph, existing records and
   draft state. Show the verified proposed changes and obtain explicit draft
   entry authorization. Reading a shared tab is not permission to change it.
5. For each field, identify the exact current accessible label, section and
   record. Compare to its official mapping and independently verified value.
   If there are several matching fields, disambiguate before typing.
6. Enter one verified value with the current form's expected decimal format.
   Do not assume a decimal separator from the user's language. Re-read the
   actual displayed field immediately; compare its normalized numeric meaning
   and record expected, observed, exercise, section and status.
7. Re-inspect after tab changes, dynamic records, navigation or session expiry.
   Stale IDs, unexpected dialogs or a read-back mismatch block further changes;
   do not repeatedly retry or silently overwrite another record.
8. Read F/G totals and reconcile with verified items. Use **Validar declaracion**
   only after confirming that the current control validates without submitting.
   Read and record every error/warning. A warning is not waived silently.
9. **Vista previa** is a draft, not a filed return (S3). Use it only when its
   current action is understood. **Guardar** saves data on AEAT's server; obtain
   separate consent before saving and confirm it saves a draft, not a return.
   Do not promise persistence without observing success.
10. Stop before **Formalizar ingreso / Devolucion**. S3 places export/payment
    options behind this stage, so export is also outside the MVP boundary. Never
    click **Firmar y enviar**, **Conforme**, payment/NRC, transfer, debt
    acknowledgement, direct-debit commitment, or upload/submission controls.
    Unknown or combined save/submit actions must not be used.

No runPlaywrightCode, DOM/script evaluation, direct network calls, bulk entry,
custom automation framework or MCP may be used. Follow the host's approvals;
the agent's instructions are not deterministic enforcement.

If browser tools are absent or blocked, provide the verified section-qualified
manual entry checklist. Ask the user for only the necessary redacted read-back
if they enter manually. Mark these values USER_PROVIDED, not agent-observed.
Do not claim live browser verification or final readiness without the required
checks. Final review always lists unresolved items and states that the return
has not been signed, submitted, or paid.