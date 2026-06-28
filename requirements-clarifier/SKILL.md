---
name: requirements-clarifier
description: >
  Activate this skill BEFORE implementing any code change, bug fix, refactor, feature request,
  UI change, database change, API integration change, or business-rule change — regardless of
  language, framework, or project type. Triggers on: implement, fix, refactor, add, change,
  update, create, delete, migrate, integrate, connect, configure, or deploy.
  This skill prevents premature or incorrect implementation by forcing a structured clarification
  and planning step before any file is modified.
  Use the lightest safe protocol. Prefer speed for trivial work and rigor for risky work.
  Do not escalate to the Full Protocol unless ambiguity, risk, scope, data, integrations,
  security, permissions, database, caching, or business logic justify it.
---

# Requirements Clarifier Skill

## Purpose

Prevent incorrect, premature, or scope-creeping changes by thinking before touching files.

Every implementation task goes through the lightest applicable protocol first.
The output is a structured plan or mini-plan before any file is modified.

The agent may inspect, read, search, and analyze files before approval.
Approval is required only before modifying, deleting, moving, generating, or executing destructive actions.

---

## Operating Principles

Use the lightest protocol that safely prevents incorrect changes. The goal is not to create paperwork;
the goal is to avoid wrong, risky, or scope-creeping implementation.

Default behavior:
- Prefer Trivial Direct Change when the user asked for an exact, harmless, reversible edit.
- Prefer Fast-Track when the change is simple but still deserves a visible mini-plan.
- Prefer Medium Protocol for narrow logic changes with limited risk.
- Use Full Protocol only when ambiguity, multi-file impact, state, permissions, data, integrations,
  caching, security, database, business rules, or destructive operations make a smaller protocol unsafe.

Do not escalate protocols just because a trigger word appears. Escalate because the change is risky,
ambiguous, broad, or hard to verify.

If inspection proves the request is simpler than expected, de-escalate to a lighter protocol.
If inspection reveals hidden risk, escalate and explain why.

---

## Classification: Trivial vs. Simple vs. Medium vs. Complex Request

Before running the full protocol, classify the request and use the lightest safe path.
The classification may be revised after inspecting files if the actual risk is higher or lower than expected.

### Trivial Direct Change
Use only when ALL of the following are true:
- The user explicitly requested a tiny, reversible change
- The exact file and replacement are clear
- No behavior, layout, data, database, API, cache, or integration impact
- No ambiguity in expected outcome

Examples: typo fix, comment wording, copy-only replacement, formatting of a single message.

Do NOT use Trivial Direct Change when the edit affects behavior, layout, runtime configuration, tests,
security, permissions, generated output, or more than one meaningful location.

For trivial direct changes, the user's request counts as approval. Make the change, then summarize:
```
**Cambio realizado:** [1 línea]
**Archivo:** [ruta exacta]
**Verificación:** [qué se revisó]
```

### Fast-Track (3-step mini-plan)
Use when ALL of the following are true:
- Single file affected, clearly identified or easily discoverable
- No database, API, cache, or integration impact
- No ambiguity in expected behavior
- Purely cosmetic, textual, presentational, or low-risk configuration change

Fast-track output format:
```
**Cambio entendido:** [1 línea]
**Archivo:** [ruta exacta]
**Cambio exacto:** [descripción precisa de qué línea o bloque cambia y por qué]
**Riesgo:** [ninguno / bajo — razón]
¿Confirmas?
```

### Medium Protocol
Use when the change affects logic but is limited, low-risk, and likely contained to one small area.
Examples: small validation change, conditional UI behavior, narrow bug fix, simple query filter,
or localized CLI behavior.

Do NOT use Medium Protocol for schema changes, auth changes, external integrations, broad refactors,
stateful UI flows, concurrency-sensitive code, financial calculations, permission logic, or destructive operations.

Medium protocol output format:
```
### Solicitud entendida
[One clear sentence restating what is being asked.]

### Archivos a revisar / modificar
- [file/path — reason]

### Supuestos
- [Assumption 1 — flagged as unconfirmed]

### Riesgos
- [Risk — mitigation]

### Plan mínimo seguro
1. [Step 1]
2. [Step 2]

### Criterios de aceptación
- [ ] [Observable outcome 1]
- [ ] [Observable outcome 2]

### Confirmación requerida
Confírmame si implemento este plan o si ajustamos el alcance primero.
```

### Full Protocol
Use for everything else: integrations, database changes, caching, API calls,
multi-file changes, business rules, data pipelines, UI with state, permissions,
security-sensitive changes, destructive operations, or anything where the impact is uncertain.

When uncertain between two levels, choose the stricter one only if the additional detail will reduce real risk.
Otherwise choose the lighter one and state the key assumption.

---

## Full Protocol — Mandatory Behavior

Before modifying any file, inspect/read/search relevant files as needed, then produce the following analysis in order.
Keep each section concise and evidence-based; omit filler and avoid repeating the same risk in multiple sections:

1. **Restate the request** in one clear sentence — what is being asked, not how.
2. **Identify known facts** from the codebase, conversation history, and context.
3. **Identify ambiguities** — anything that could be interpreted two ways.
4. **List assumptions** — what you are taking as true without confirmation.
5. **Define scope** — exactly what will change.
6. **Define out of scope** — what will NOT change, even if it looks related.
7. **List files to inspect** before writing code.
8. **List files likely to change.**
9. **Map affected rules** — business logic, data rules, UI behavior, permissions,
   calculations, integrations, caching, or external API contracts.
10. **Identify risks** — regressions, data loss, API breaking changes, token expiry,
    cache invalidation, race conditions, concurrency issues.
11. **Propose the minimum safe plan** — smallest change that satisfies the requirement.
12. **Define acceptance criteria** — observable, testable outcomes.
13. **Define manual verification steps** — how to confirm it works in the browser or CLI.
14. **Request explicit approval** before touching files.

---

## Evidence Hierarchy

When planning, prefer evidence in this order:

1. Code currently in the repository.
2. Existing tests, schemas, migrations, API contracts, config examples, and documentation in the repository.
3. User-provided requirements in the current conversation.
4. Reasonable assumptions clearly marked as unconfirmed.

Never let assumptions override visible code, tests, schemas, or explicit user requirements.
If repository evidence contradicts the user request, pause and flag the contradiction before implementation.

---

## Command Safety

Before approval, safe read-only commands are allowed: file search, grep, listing directories, opening files,
static inspection, and non-mutating diagnostics.

Before approval, do not run commands that modify files, install dependencies, apply migrations, write caches,
format code, regenerate artifacts, start long-running services, or mutate external systems.

After approval, run only commands relevant to the approved plan. Prefer targeted checks before broad ones.

---

## Do Not

- Do not modify files before approval.
- Do not invent business rules not stated or visible in the code.
- Do not assume hidden requirements.
- Do not expand scope without approval.
- Do not refactor code unrelated to the request.
- Do not change database schema unless explicitly requested.
- Do not edit `.env`, credentials, production config, vendor files, generated files,
  backups, logs, or database dumps unless explicitly requested.
- Do not run destructive database commands (`DROP`, `TRUNCATE`, `DELETE` without `WHERE`).
- Do not delete files or change file permissions without approval.
- Do not replace a working integration pattern (e.g., hardcoded JWT, static endpoint)
  without flagging it as a deliberate architectural decision and getting approval.
- Do not silently change API versions (e.g., v2 → v3) without noting the reason and risk.
- Do not produce long plans for low-risk work when a lighter protocol is sufficient.
- Do not ask questions that can be answered by inspecting the repository.

---

## When to Ask Questions First

If an ambiguity could reasonably lead to different code changes, behavior, files modified,
business rules, UI behavior, data handling, permissions, API behavior, test expectations,
or verification steps, ask one interactive clarification block BEFORE producing the implementation plan.

Do not convert implementation-changing ambiguities into assumptions unless the user explicitly asks for speed,
or the safest default is obvious, reversible, and low-risk.

Use this clarification format:

```
### Necesito aclarar algo antes del plan

**Decisión:** [what decision must be made before planning]

**Opciones:**
1. [Option A — technical consequence]
2. [Option B — technical consequence]
3. [Option C — technical consequence]
4. Otra — te explico exactamente lo que quiero

**Recomendación:** [safest or most maintainable default, with reason]

**Impacto:** [what files, behavior, risks, tests, or verification steps could change depending on the answer]

Respóndeme con el número de opción o con tu propia instrucción.
```

Only skip this clarification block when the ambiguity is trivial, reversible, does not affect implementation,
or can be safely resolved by inspecting the repository.

Ask first if any of the following are blocking. If a question is useful but not implementation-changing,
place it under assumptions or risks instead of stopping.

**Repository Evidence**
- Can the answer be found by inspecting files, tests, schemas, or docs? If yes, inspect first instead of asking.
- Did inspection reveal contradictory patterns that make the requested change unsafe or unclear?

**Behavior**
- What is the current behavior (broken or baseline)?
- What is the exact expected behavior after the change?
- Does this affect historical data or only new records going forward?

**Scope**
- Which screen, route, report, export, job, API endpoint, or process is affected?
- Which user roles or permission levels are affected?
- Is this a one-time fix or a recurring pattern to standardize?

**Data**
- What is the data source (DB table, API endpoint, XML feed, CSV, session)?
- What filters, date ranges, statuses, or conditions apply?
- Are there rounding, currency, unit, or precision rules?
- What happens with null, empty, or malformed data?

**Integrations**
- Which external service, API, or third-party dependency is involved?
- What authentication method does it use (API key, OAuth, static token, JWT)? Could it have expired?
- Which endpoint and version is being called? Is that version confirmed to support the required operation?
- Is the integration under a subscription or plan that limits available endpoints?
- Are there required fields, headers, or payload constraints imposed by the external service?
- Is there a known reliability issue with this service that requires a fallback strategy?

**Database**
- Which tables, columns, or indexes are affected?
- Are there foreign keys, triggers, or stored procedures that could be impacted?
- Will existing rows need migration or backfill?

**Caching**
- Is there a cache (file, APCu, Redis, HTTP) that needs invalidation after this change?
- What is the TTL and does it match the expected data freshness?

**Acceptance**
- What does "done" look like? What will the user see, click, or receive?
- Are there edge cases that must be explicitly handled?

---

## Plan Quality Gate

Before asking for approval, verify that the plan is:

- Minimal: it changes only what is required.
- Testable: acceptance criteria are observable, not vague.
- Bounded: out-of-scope items are explicit when risk exists.
- Reversible: risky changes include rollback or mitigation where practical.
- Evidence-based: file paths, rules, and risks come from inspection or are marked as assumptions.

If the plan fails any of these, revise it before presenting it.

---

## Universal Risk Checklist

Before finalizing the plan, mentally verify each applicable item.
Skip sections that don't apply to the current stack or context.

**Security & Input Handling**
- [ ] Are all external inputs validated and sanitized (injection, XSS, path traversal)?
- [ ] Are parameterized queries or prepared statements used for every DB operation?
- [ ] Is sensitive output suppressed in production (no debug dumps, stack traces, raw errors)?
- [ ] Are file paths constructed safely without user-controlled segments?

**APIs & Integrations**
- [ ] Is the auth token/key stored in config or environment — not hardcoded?
- [ ] Is the HTTP response code checked before consuming the response body?
- [ ] Is there a timeout and a retry or fallback policy for the external call?
- [ ] Could the token or credential have expired? Is renewal manual or automatic?
- [ ] Is the correct API version being used? Is the endpoint available in the active subscription?
- [ ] Does the external service have known reliability issues requiring a fallback?

**Caching**
- [ ] Will stale cache serve wrong data after this change?
- [ ] Is the cache key scoped correctly so different entities don't collide?
- [ ] Is the TTL appropriate for the expected data freshness?
- [ ] Does a full cache invalidation risk generating a spike of upstream API calls?

**Database**
- [ ] Does every mutating query have a `WHERE` clause where one is expected?
- [ ] Are joins performed on indexed columns?
- [ ] Could this query be slow on large datasets? (Run `EXPLAIN` if uncertain.)
- [ ] Are there foreign keys, triggers, or cascades that could have side effects?
- [ ] Do existing rows need migration or backfill after a schema change?

**Logic & Business Rules**
- [ ] Does this change silently alter a calculation, total, status, or threshold used elsewhere?
- [ ] Are edge cases handled: null values, empty arrays, zero amounts, missing fields?
- [ ] Does the change affect historical data or only new records?
- [ ] Are there downstream processes, reports, exports, or notifications that depend on this logic?

---

## Output Format (Full Protocol)

```
### Solicitud entendida
[One clear sentence restating what is being asked.]

### Hechos conocidos
- [Fact from code or context]
- [...]

### Ambigüedades o preguntas
- [Question 1]
- [Question 2 — leave blank if none]

### Supuestos
- [Assumption 1 — flagged as unconfirmed]
- [...]

### Alcance propuesto
- [What will change, file by file if possible]

### Fuera de alcance
- [What will NOT change]

### Archivos a revisar antes de implementar
- [file/path — reason to inspect]

### Archivos posiblemente a modificar
- [file/path — why]

### Reglas de negocio / integraciones involucradas
- [Rule or integration and how it is affected]

### Riesgos
- [Risk — likelihood — mitigation]

### Plan mínimo seguro
1. [Step 1]
2. [Step 2]
...

### Criterios de aceptación
- [ ] [Observable outcome 1]
- [ ] [Observable outcome 2]

### Pasos de verificación manual
1. [What to do in browser / CLI / DB to confirm it works]
2. [Edge case to test]

### Confirmación requerida
Confírmame si implemento este plan o si ajustamos el alcance primero.
```


---

## After Approval

Once the user approves the plan:

1. Inspect the listed files first if not already inspected.
2. If findings contradict the approved plan, stop and present an updated plan for approval.
3. Make the smallest necessary change that satisfies the requirement.
4. Avoid unrelated refactors, formatting churn, or opportunistic cleanup.
5. Run the most relevant available tests, linters, type checks, build commands, or manual checks.
6. If tests cannot be run, explain why and provide the safest manual verification path.
7. If checks fail, fix issues within the approved scope. If fixing requires new scope, stop and ask.
8. Summarize the actual changes, files modified, verification performed, and any remaining risks.

Post-implementation summary format:
```
### Cambios realizados
- [file/path — what changed]

### Verificación
- [command/check performed — result]

### Riesgos o pendientes
- [remaining risk, limitation, or "ninguno conocido"]
```

---

## Approval Rule

**Only modify files after the user explicitly approves the plan, except for Trivial Direct Changes.**

Acceptable confirmations: "sí", "adelante", "confirmo", "go ahead", "hazlo", "implementa".

If the user’s latest message already includes explicit implementation authorization, such as
"hazlo", "implementa esto", or "aplica el cambio", treat it as approval only for Trivial Direct
Changes. For Fast-Track, Medium, and Full Protocol, still present the appropriate plan first and
request confirmation unless the user has already approved that specific plan.

If the user modifies the plan in their reply, re-output only the changed parts of the updated plan
and ask again before touching files.

If the user asks to skip confirmation permanently, do not disable this rule. Instead, continue using
Trivial Direct Change for harmless exact edits and approval-gated protocols for anything risky.

---

## Decision Shortcut

Use this quick rule when choosing a protocol:

- Exact harmless edit → Trivial Direct Change.
- Cosmetic or copy/config tweak with tiny risk → Fast-Track.
- Localized logic or bug fix with limited blast radius → Medium Protocol.
- Anything involving data correctness, auth, payments, permissions, APIs, DB, caching, migrations,
  multi-file architecture, destructive operations, or unclear business rules → Full Protocol.

When the protocol feels heavier than the risk, downgrade.
When the risk feels larger than the protocol, upgrade.

---

## Anti-Patterns to Avoid

| Razonamiento peligroso | Por qué es incorrecto |
|---|---|
| "Es un cambio pequeño, lo hago directo" | Los cambios pequeños rompen integraciones ocultas |
| "Asumo que el token / credencial sigue válido" | Los tokens expiran; la renovación puede ser manual |
| "Cambio la versión de API porque es más nueva" | Las versiones más nuevas pueden no tener los mismos endpoints |
| "Invalido el caché completo para asegurar" | Puede generar una avalancha de llamadas al servicio externo |
| "El campo está vacío, lo elimino del payload" | Algunos servicios requieren el campo aunque esté vacío |
| "El fallback no es necesario, la API es estable" | Ninguna API externa garantiza disponibilidad total |
| "Refactorizo esto de paso, está mal hecho" | El refactor fuera de alcance introduce riesgo sin aprobación |
| "No necesito revisar el esquema, solo es un SELECT" | JOINs en columnas sin índice pueden tumbar queries en producción |
| "Asumo que el comportamiento esperado es obvio" | Las reglas de negocio implícitas son la causa más común de bugs |
| "Uso Full Protocol para todo por seguridad" | La fricción excesiva hace que el proceso deje de usarse |
| "Pregunto antes de revisar el código" | Muchas dudas se resuelven inspeccionando archivos existentes |
| "La aprobación inicial cubre cualquier hallazgo nuevo" | Riesgos descubiertos después de inspeccionar requieren replantear el plan |
