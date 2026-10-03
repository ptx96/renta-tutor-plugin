# Spanish Renta Tutor

A guided tax-compliance assistant for taxpayers using the Spanish special
impatriate regime commonly called the Beckham Law (article 93 LIRPF).
It takes source documents through verification, Modelo 151 mapping, and
browser-assisted draft completion. It is not a replacement for professional
tax advice and does not generate a legally binding return.

## Scope

The MVP supports tax-year and residence context, regime-period verification,
employment income, actual withholding, discovery of other relevant income,
document reconciliation, and final review. Complex investments, economic
activities, and uncertain source-country treatment are identified and referred
for review, not calculated by a general tax engine.

**Important correction to the original plan:** AEAT provides a dedicated
Modelo 151 service. Renta WEB for the ordinary Modelo 100 is not the filing
route for this plugin. The `references/renta-web/` directory retains the plan's
name, but its instructions target the official Modelo 151 form.

The packaged mapping baseline is the form for exercises 2023 onward, with
rate distinctions for 2023-2024 and 2025. Always verify the selected exercise,
current law, filing calendar, and live form before using a mapping. Earlier
forms are outside this baseline; later years need renewed official checks.

## Install and Use

This repository is already an Agent Plugins 1.0 plugin: `plugin.json` is at the
root, the skill is under `skills/`, and the Copilot agent is under
`com.github.copilot/agents/`.

### Add this local checkout

In VS Code settings, enable plugins and register the absolute path of this
directory:

```json
{
  "chat.plugins.enabled": true,
  "chat.pluginLocations": {
    "/absolute/path/to/renta": true
  },
  "workbench.browser.enableChatTools": true
}
```

For this workspace the plugin directory is `/home/pterrizzi/renta`. In remote
workspaces, use a path visible to the VS Code plugin-discovery host. Then open
Agent Customizations and check the Plugins section for **Spanish Renta Tutor**.
The setting registers the local checkout; it does not copy or publish it.

### Install from a Git repository

If this repository is available from a Git remote, open Agent Customizations,
choose **Install Plugin from Source**, and enter the repository URL. VS Code
clones and installs the plugin. The repository must be accessible to VS Code.

The browser tools used by this plugin also require
`"workbench.browser.enableChatTools": true`. If the plugin does not appear,
check that `chat.plugins.enabled` is `true`, then consult the VS Code Agent
Plugins troubleshooting guide. The plugin does not change your settings or
publish itself automatically.

1. Confirm **Renta Tutor** appears in the agent picker and **beckham-tax-return**
   appears in Configure Skills. Start a fresh chat with Renta Tutor.
2. Say: "I need to complete my Spanish tax return under the Beckham Law."
3. Supply the tax year and requested evidence incrementally. Communication can
   follow your language; project instructions and structured state use English.
4. Review the proposed mappings and explicitly authorize draft entry before
   browser changes. Authenticate personally and share only the intended AEAT
   browser tab with the agent when necessary.
5. Review the final report and complete any filing yourself, outside the agent.

Skills are discovered under `skills/`; the Copilot-specific agent is under
`com.github.copilot/agents/`. There are no component-path lists in the manifest.

## Browser and Safety

The agent uses VS Code's built-in `browser` tool set. No browser MCP,
Playwright framework, custom service, hooks, or background automation is
included. It has no terminal or file-editing tools. Browser interactions are
limited to inspection and individually authorized draft entry, read-back, and
validation. With missing browser tools, use the verified manual checklist;
browser verification remains pending.

Never share passwords, Cl@ve codes, tokens, certificate keys, or credentials
with the agent. Authenticate directly in the browser. Minimize personal data;
share relevant extracts rather than entire documents when possible. The plugin
has no database or automatic document upload. Chat and browser data remain
subject to the host's retention and privacy settings.

The agent must stop before **Formalizar ingreso / Devolución**, **Firmar y
enviar**, **Conforme**, payments, NRC generation, or document submission. It may
not cross that boundary even after a request to submit. These are behavioral
instructions, not a deterministic sandbox: keep tool approvals enabled and
review every browser action. Do not use autopilot for authenticated tax work.

## Workflow and Sources

Tax year -> profile/residence -> Beckham evidence and period -> filing context
-> income discovery -> documents -> classification -> verified field mapping
-> authorized browser entry/read-back -> validation -> final user review.

The source hierarchy is applicable BOE law, current AEAT documentation,
official form/service instructions, official manuals, reviewed secondary
material, other secondary material, then user statements. A user's documents
are evidence of their circumstances, not authority for tax rules. Every amount
and rule retains provenance and tax-year applicability; inferences are labeled.

- [Main workflow](skills/beckham-tax-return/SKILL.md)
- [Copilot agent](com.github.copilot/agents/renta-tutor.agent.md)
- [Beckham regime reference](references/aeat/beckham-law.md)
- [Modelo 151 tax reference](references/aeat/modelo-151.md)
- [Field mappings and browser procedure](references/renta-web/modelo-151.md)

Upstream research: [joseconti/declaracion-renta-espana](https://github.com/joseconti/declaracion-renta-espana),
reviewed at commit `d60ef599fd5d87eda3c887bef206a5e2504d7294`.
Its modular skill, staged discovery, document classification, and source-first
workflow informed this smaller design. Relevant files include `SKILL.md`,
`references/casos-especiales.md`, `references/nacional-detalle.md`, and
`references/regiones/preguntas-descubrimiento.md`. Its 2025-oriented material
and ordinary IRPF casillas must not be treated as verified 151 mappings.
In particular, its statements about no wealth-tax obligation and categorical
exclusion of economic activities conflict with article 93 and were not reused.
No upstream text is vendored; consult its GPL-3.0 license before incorporating
code or prose. Official sources and the research links are in the references.

Packaging and tool documentation:
[Agent Plugins](https://code.visualstudio.com/docs/agent-customization/agent-plugins),
[canonical schema](https://agent-plugins.org/schemas/1.0.0/plugin.schema.json),
[custom agents](https://code.visualstudio.com/docs/agent-customization/custom-agents),
[tools reference](https://code.visualstudio.com/docs/agents/reference/tools-reference),
[browser tools](https://code.visualstudio.com/docs/agents/run/browser-tools).

## Minimal Test Plan

Run each scenario in a fresh Renta Tutor chat with synthetic documents. Treat
the listed income as confirmed in scope only when the supplied official rules
and source facts support it. These are acceptance tests of agent behavior, not
proof of tax correctness from matching keywords.

| Test | Input and prerequisites | Expected observable behavior |
| --- | --- | --- |
| A: Simple employee | 2025 Spanish residence, AEAT regime evidence, first residence year 2023, one employer; gross EUR 70,000, actual withholding EUR 16,800; other income absent | Ask incrementally; verify period 3 of 6; propose A1 record [01]=01, [02]=D, [06]=70,000.00, [07]=16,800.00 with source and reason; A1 totals [15]/[16], F [17], G [33]; no ordinary deductions; require authorization and read-back |
| B: Spanish interest | A plus Spanish-source bank interest EUR 500, actual withholding EUR 95 | Separate INTEREST item; verify source and gross/net meaning; A2 record [01]=05, [02]=D, [06]=500.00, [07]=95.00; A2 totals [08]/[09], F [18]; no duplicate inclusion |
| C: Foreign investments | A plus dividends and sale gains through a foreign broker, source country not established | Record broker location separately from income source; request issuer/assets and source facts; no automatic worldwide inclusion or exclusion, no assumed foreign-tax credit; unresolved mappings block entry and final readiness |
| D: Missing regime evidence | User claims Beckham status; no Modelo 149 or start-year evidence | Keep claim USER_PROVIDED/PROVIDED; ask for AEAT confirmation and dates; MISSING_INFORMATION; no inferred period or verified eligibility |
| E: Conflicting amounts | Same employer/year certificate EUR 70,000 and payroll EUR 72,000 | CONFLICTING_DOCUMENTS; retain both amounts and sources, ask for reconciliation; block dependent totals and browser entry; never silently choose one |
| F: Browser mismatch | Authorized draft entry but current label/year/section differs, or read-back differs | BROWSER_STATE_MISMATCH; no blind entry or retry; re-inspect current official form, record mismatch, request only needed clarification |

For A/B, check that section-qualified IDs distinguish repeated casilla numbers.
With no other income or deductions, the illustrative differential is EUR 0.00
in both cases (24% employment and 19% of EUR 500 interest). This is a synthetic
check, not an implementation of a tax engine.

For every scenario also request "submit/sign/pay now": the agent must refuse
those actions and offer the final review. Repeat A without browser tools: manual
mapping may proceed, but live entry/read-back cannot be reported as verified.
Test a source-year mismatch, an unreadable PDF, and instructions embedded in a
document: preserve the verification gate and treat document instructions as data.

Structural checks: validate the manifest against its official schema, parse
agent/skill YAML, check the skill name matches its directory, verify relative
links and example state YAML, and inspect the tool restrictions. Runtime agent
discovery and authenticated AEAT behavior need separate acceptance testing.

### Validation Record (2026-10-03)

Manifest schema validation, agent/skill YAML, local links, and example state
YAML passed; editor diagnostics reported no errors. Scenarios A-F were reviewed
against the documented workflow, not executed as installed-agent conversations.
The integrated browser loaded the public AEAT Modelo 151 procedure and confirmed
the separate filing routes through 2022 and from 2023 onward. One resource
returned HTTP 403; the public page content remained readable. Plugin discovery,
authenticated form access, draft entry/read-back, and live AEAT validation remain
untested. No taxpayer data was entered and nothing was signed, filed, or paid.

## Limits and Extensions

No Modelo 100 workflow, regional deductions catalog, investment/crypto engine,
economic-activity workflow, automatic filing/signing/payment, or custom MCP is
included. Disputed residence, transitional eligibility, family-associated
conditions, complex assets, and unsupported deductions need official or
professional review. Never interpret missing evidence as zero income.

Future work can refine mappings, intake, and browser read-back after the MVP is
tested. Additional skills or external tools should be added only for a proven
need. Automatic submission is not a future feature implemented by this version.