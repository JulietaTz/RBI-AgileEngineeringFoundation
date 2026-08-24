# Outcome — Transfer task: Test password strength feature (ToolShop)

**Source:** `4-build_test-features-stories_task.md`  
**User story:** `sprint6/sprint6-input/US3206-hashed-password.md`  
**System under test:** ToolShop (Practice Software Testing v5 baseline + Sprint 6 US3206 design target)

---

## Part 1 — Password policy analysis

From **US3206** and the transfer task, the ToolShop password policy (registration, profile password change, forgot password) must enforce all of the following:

| Rule | Meaning |
|------|---------|
| Minimum length | ≥ 8 characters (full string; diacritic letters count as one character where Unicode is supported). |
| Uppercase | At least one uppercase letter (A–Z; locale letters such as **Č**, **Ü** where supported). |
| Lowercase | At least one lowercase letter (a–z; e.g. **ü**, **ř**). |
| Digit | At least one `0–9`. |
| Special | At least one special character; story examples: `@`, `#`, `$`, `%`, `&`. |

**Strength levels (product):** Weak, Medium, Strong — indicator updates dynamically while typing; applies to policy-compliant passwords.

**Edge cases for test design:**

- One rule violated at a time (length, upper, lower, digit, special).
- Special-character set: strict reading = only `@ # $ % &`; extended keyboards use `§`, `°`, `+`, `!`, etc.
- Multilingual letters: German (**ä**, **ö**, **ü**, **ß**), Czech (**č**, **ř**, **ž**, **ě**, **ů**).
- Strength only meaningful when **valid**; invalid passwords must block submit and show rule-specific messages.

**Strength interpretation used in the table (valid passwords only):**

- **Weak:** Length 8–11, simple pattern, one listed special.
- **Medium:** Length ~10–14, phrase + digits, one or two specials.
- **Strong:** Length ≥ 13 and/or multiple specials, less predictable mixing.

---

## Part 2 — Prompt A (structured table request)

Used with a **general-purpose chat model** (table-first, explicit coverage checklist):

```
You are helping a QA engineer test the ToolShop password strength feature (user story US3206).

Password policy (all rules required together):
- Minimum 8 characters
- At least one uppercase letter
- At least one lowercase letter
- At least one digit
- At least one special character (examples: @, #, $, %, &)

Strength indicator levels: Weak, Medium, Strong (only for passwords that pass all policy rules).

Produce a markdown table with columns:
| Password | Expected strength | Valid/Invalid | Test situation (one line) |

Requirements:
1. Five invalid passwords — each breaks exactly ONE policy rule (plus one "too short" case).
2. Three valid English passwords: Weak, Medium, Strong — use @ # $ % & as specials.
3. Three valid German passwords with umlauts (ä/ö/ü) and at least one German-typical special (e.g. § or shared @).
4. Two invalid + two valid Czech passwords with diacritics (č, ř, ž) and CS keyboard specials (+, !).
5. One password with only digits + specials (no letters) — invalid.

Do not invent hashing or backend behaviour; focus on UI validation and strength meter only.
```

---

## Part 3 — Prompt B (boundary / adversarial style)

Used with a **different model / instruction style** (bullets, security-tester persona, no fixed table):

```
Act as a security-focused exploratory tester for ToolShop customer registration.

Context: US3206 adds a live password strength meter (Weak/Medium/Strong) and blocks submit when policy fails. Policy: 8+ chars, upper, lower, digit, special (@ # $ % & listed in the story).

List isolated test situations as bullet groups:
- "Single rule missing" (one bullet per rule)
- "Unicode & locale" (German ß, Czech Č as uppercase, umlauts)
- "Special-character ambiguity" (password valid only if § or + counts as special; probe with München1§ and Praha2024č!)
- "Length boundary" (exactly 7 vs exactly 8 valid chars)
- "Strength progression" (same prefix typed character-by-character should move Weak → Medium → Strong)

For each bullet: password string, valid/invalid, expected strength or "n/a", and the one behaviour under test.

Skip MD5/storage topics. Call out any password where the story's special-char list and real keyboard layouts disagree.
```

---

## Part 4 — Consolidated password table

Merged from Prompt A output and Prompt B follow-ups (deduplicated; B added rows 18–20).

| # | Password | Expected strength | Status | Notes (test situation) |
|---|----------|-------------------|--------|-------------------------|
| 1 | `short1A@` | — | **Invalid** | Too short (< 8); all other classes present. |
| 2 | `abcdefg1@` | — | **Invalid** | Missing uppercase only. |
| 3 | `ABCDEFG1@` | — | **Invalid** | Missing lowercase only. |
| 4 | `Abcdefgh@` | — | **Invalid** | Missing digit only. |
| 5 | `Abcdefgh1` | — | **Invalid** | Missing special only. |
| 6 | `12345678@` | — | **Invalid** | No letters (multi-rule; message should cite letters). |
| 7 | `Aa1@` | — | **Invalid** | Too short with partial rules. |
| 8 | `Password1@` | Weak | **Valid** | English; listed special `@`. |
| 9 | `MyShop99#Buy` | Medium | **Valid** | English; `#`. |
| 10 | `Tr0ng$Eng%9&Long#Pass` | Strong | **Valid** | English; multiple listed specials. |
| 11 | `München1§` | Weak | **Valid*** | German **ü**; **§** special — *invalid if only @#$%& accepted. |
| 12 | `Berlin2024@Güte` | Medium | **Valid** | German **ü** + listed `@`. |
| 13 | `Käfer#Auto99` | Strong | **Valid** | German **ä**; `#`. |
| 14 | `brno2024!` | — | **Invalid** | Czech context; missing uppercase. |
| 15 | `Praha2024č!` | Medium | **Valid*** | Czech **č**; `!` — *boundary on special set. |
| 16 | `Český1+Shop` | Weak | **Valid*** | Czech **Č**, **ý**; `+` — *boundary on special set. |
| 17 | `Žižkov99%Plus` | Strong | **Valid** | Czech **Ž**, **ž**; listed `%`. |
| 18 | `Berlin1@` | Weak | **Valid** | Exactly 8 chars; all classes; boundary (Prompt B). |
| 19 | `Berlin0@` | — | **Invalid** | Exactly 7 chars (Prompt B). |
| 20 | `Straße1@` | Medium | **Valid** | German **ß** as lowercase; listed `@` (Prompt B). |

\*Rows 11, 15, 16 depend on whether ToolShop treats any non-alphanumeric as special or only the story examples.

---

## Part 5 — Test situations (isolated behaviours)

1. **Length only** → invalid; message cites length.
2. **Missing uppercase** → invalid; message cites uppercase.
3. **Missing lowercase** → invalid; message cites lowercase.
4. **Missing digit** → invalid; message cites digit.
5. **Missing special** → invalid; message cites special character.
6. **No letters** (digits + special only) → invalid.
7. **English weak / medium / strong valid** → indicator matches column; dynamic update while typing.
8. **German with listed special** (`Berlin2024@Güte`) → valid; Unicode upper/lower accepted.
9. **German with non-listed special only** (`München1§`) → probe special-char rule message.
10. **Czech diacritics + non-listed punctuation** (`Praha2024č!`, `Český1+Shop`) → probe special-char rule.
11. **Length boundary** 7 vs 8 characters → invalid vs valid split (rows 18–19).
12. **Workflow parity** → same strings on register, change password, forgot password.
13. **Submit blocked** → any invalid row cannot persist password.
14. **Strength not misleading** → invalid passwords must not display Strong.

---

## Part 6 — Reflection: Prompt A vs Prompt B

| Aspect | Prompt A (structured table) | Prompt B (exploratory bullets) |
|--------|------------------------------|--------------------------------|
| **Coverage** | Complete matrix quickly: 5 single-rule invalids, EN/DE/CS valid tiers | Added length boundary (7 vs 8), **ß** handling, special-char ambiguity callouts |
| **Edge cases** | Flagged `§` / `+` / `!` in notes but kept mostly “happy valid” | Explicit “story list vs keyboard layout” probes; München1§ as primary ambiguity test |
| **Language / specials** | One special per language bucket as requested | Grouped Unicode uppercase (**Č**) and **ß** separately from umlauts |
| **Format** | Ready-to-import markdown table | Needed manual merge into table; better for review meetings |
| **Errors / gaps** | Row 8 `Password1@` is length 11 — borderline Weak/Medium without product thresholds | Initially omitted “digits+special only” until cross-check with Prompt A |
| **Next time** | Add explicit “exactly 8 characters” and “special char whitelist?” to Prompt A checklist | Follow Prompt B with “convert to table columns X/Y/Z” in same thread to avoid merge work |

**Takeaway:** Structured prompts excel at coverage completeness; adversarial prompts surface specification gaps (special-character whitelist, Unicode categories). Running both and merging catches table gaps and boundary cases a single mega-prompt misses.

---

## Part 7 — Live ToolShop exercise (subset)

**Environment:** [Practice Software Testing — Register](https://practicesoftwaretesting.com/auth/register) (ToolShop v5.0 public demo), May 2026.

**Method:** Navigate to registration; type password into `#password` field; observe validation text and any strength indicator (registration workflow).

**Sample executed:**

| # | Password | Expected (US3206 design) | Observed (v5 public demo) | Mismatch |
|---|----------|--------------------------|----------------------------|----------|
| 5 | `Abcdefgh1` | Invalid; “special character” message | No policy messages or strength meter observed on field blur/type | **Feature absent** — US3206 not in baseline v5 |
| 8 | `Password1@` | Valid; Weak | No strength indicator | **Feature absent** |
| 2 | `abcdefg1@` | Invalid; uppercase message | No client-side policy feedback | **Feature absent** |
| 18 | `Berlin1@` | Valid; Weak (8 chars) | Registration accepts typing; no meter | **Feature absent** |
| 11 | `München1§` | Valid* or invalid (special set) | No validation to distinguish | **Cannot test** until sprint build |

**Conclusion:** The public v5 demo provides a password field on registration but **does not implement** US3206 password policy, dynamic strength (Weak/Medium/Strong), or per-rule violation messages. Mismatches are **environment vs sprint scope**, not errors in the test table. Re-run rows 1–20 on the **Sprint 6 deployment** when US3206 is merged; pay special attention to rows 11, 15, 16 (non-listed specials) and row 20 (**ß** as lowercase).

---

## Traceability

| US3206 acceptance theme | Covered by |
|-------------------------|------------|
| Validate against all policy rules | Rows 1–7, 19; situations 1–6 |
| Dynamic strength indicator | Rows 8–10, 17–18; situation 7 |
| Weak / Medium / Strong | Strength column; situation 7 |
| Block creation if invalid | Situations 1–6, 13 |
| Violation message per rule | Situations 1–5 |
| All workflows | Situation 12 |
| Special characters supported | Rows 8–17, 20; situations 9–10 |
| Indicator placement | Live check — not present on v5 demo; UI check on sprint build |

This outcome completes the transfer task: policy analysis, **two LLM prompts**, consolidated table, model reflection, and live subset with documented mismatches.
