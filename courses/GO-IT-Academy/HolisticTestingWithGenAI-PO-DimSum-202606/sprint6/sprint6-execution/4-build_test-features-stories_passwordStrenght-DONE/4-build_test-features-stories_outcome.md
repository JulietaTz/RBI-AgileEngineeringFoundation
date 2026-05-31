# Outcome — Transfer task: Test password strength feature (ToolShop)

**Source:** `4-build_test-features-stories_task.md`  
**User story:** `sprint6/sprint6-input/US3206-hashed-password.md`  
**Note:** Design-only; no live ToolShop environment was exercised.

---

## Part 1 — Password policy analysis

The ToolShop policy (registration, profile password change, forgot password) must enforce all of the following at once:

| Rule | Meaning |
|------|---------|
| Minimum length | ≥ 8 characters (counting the full string, including specials and letters with diacritics as one character each where Unicode is supported). |
| Uppercase | At least one character with Unicode category “uppercase letter” (A–Z and locale letters such as **Č**, **Ü** where supported). |
| Lowercase | At least one “lowercase letter” (a–z and e.g. **ü**, **ř**). |
| Digit | At least one `0–9`. |
| Special | At least one non-alphanumeric symbol (policy examples: `@`, `#`, `$`, `%`, `&`; extended sets below for DE/CS keyboards). |

**Implication for testing:** Each rule is an isolated violation surface: five “single-rule missing” families plus “too short” and “empty”. Dynamic strength (Weak / Medium / Strong) applies only after the string is **valid** under the policy; invalid passwords should still drive the indicator and messages toward failure, and submission must remain blocked per acceptance criteria.

**Character sets for three languages (specials used in samples):**

| Language | Typical letters in examples | Special characters used (beyond @ # $ % &) |
|----------|------------------------------|-----------------------------------------------|
| **English** | A–Z, a–z | `@`, `#`, `$`, `%`, `&` |
| **German** | Includes **ä**, **ö**, **ü**, **ß** | `§`, `°` (plus shared `@` / `#` where noted) |
| **Czech** | Includes **č**, **ř**, **ž**, **ě**, **ů** | `+`, `!` (ASCII punctuation common on CS layouts; combined with diacritic letters) |

Invalid examples deliberately break exactly one (or length) rule where possible so failures are attributable.

**Strength interpretation used for the table (for valid passwords only):**

- **Weak:** Valid policy, length 8–9, single simple special, predictable pattern (e.g. `Password1@`).
- **Medium:** Length about 10–12, mix of words/numbers, one or two specials.
- **Strong:** Length ≥ 13 and/or several specials, less predictable mixing (still policy-compliant).

*(Exact Weak/Medium/Strong thresholds may be tuned in the product; this set is consistent for test design and review.)*

---

## Part 2 — Password set and expected results

### Table: password, expected strength, status

| # | Password | Expected strength | Status | Notes (test situation) |
|---|----------|-------------------|--------|-------------------------|
| 1 | `short1A@` | — | **Invalid** | Too short (< 8). |
| 2 | `abcdefg1@` | — | **Invalid** | No uppercase. |
| 3 | `ABCDEFG1@` | — | **Invalid** | No lowercase. |
| 4 | `Abcdefgh@` | — | **Invalid** | No digit. |
| 5 | `Abcdefgh1` | — | **Invalid** | No special character. |
| 6 | `12345678@` | — | **Invalid** | No letters. |
| 7 | `Aa1@` | — | **Invalid** | Too short with otherwise partial rules. |
| 8 | `Password1@` | Weak | **Valid** | English set; minimal complexity, length 11. |
| 9 | `MyShop99#Buy` | Medium | **Valid** | English; `#`; typical phrase + digits. |
| 10 | `Tr0ng$Eng%9&Long#Pass` | Strong | **Valid** | English; multiple specials `%`, `&`, `#`, `$`. |
| 11 | `München1§` | Weak | **Valid** | German **ü**; special **§**; digit; mixed case. |
| 12 | `Berlin2024@Güte` | Medium | **Valid** | German **ü** + policy-listed `@`. |
| 13 | `Käfer#Auto99` | Strong | **Valid** | German **ä**; `#`; longer mixed token. |
| 14 | `brno2024!` | — | **Invalid** | Czech context letters but **no** uppercase. |
| 15 | `Praha2024č!` | Medium | **Valid** | Czech **č**; `!` special; digit; case mix. |
| 16 | `Český1+Shop` | Weak | **Valid** | Czech **Č**, **ý**; `+` special; digit. |
| 17 | `Žižkov99%Plus` | Strong | **Valid** | Czech **Ž**, **ž**; `%` special; longer. |

**Boundary note:** If the implementation treats only `@ # $ % &` as “special” (strict reading of the story examples), strings that rely on other symbols—e.g. German **§** (`München1§`), Czech **`+`** / **`!`**—may be **invalid**; record actual behaviour and align messages with the deployed rule set. Similarly, `Berlin2024°Güte` (**°** only) is a useful probe for Unicode-only punctuation.

---

## Part 3 — Test situations (isolated behaviours)

Each row below is one **test situation** (one focused condition on the ToolShop password field / flow).

1. **Length only:** Password shorter than 8 characters with all other character classes present → **invalid**; message cites length; strength does not imply “Strong”.
2. **Missing uppercase:** All other classes present → **invalid**; message cites uppercase rule.
3. **Missing lowercase:** → **invalid**; message cites lowercase rule.
4. **Missing digit:** → **invalid**; message cites numeric rule.
5. **Missing special:** → **invalid**; message cites special-character rule.
6. **Letters missing:** Only digits and specials → **invalid** (multiple violations possible; expect clear messaging order per UX).
7. **English weak valid:** e.g. `Password1@` → **valid**, indicator **Weak**, submit allowed only when all workflows agree.
8. **English medium / strong valid:** progressive typing shows indicator moving **Weak → Medium → Strong** (dynamic update AC).
9. **German diacritics + listed special:** e.g. `Berlin2024@Güte` → **valid** if Unicode letters accepted as upper/lower.
10. **Czech diacritics + punctuation special:** e.g. `Praha2024č!` → **valid** same assumption as (9).
11. **Special-character set boundary:** If only `@ # $ % &` count as specials, passwords using only other symbols (e.g. German **°**) must be **invalid** with a clear rule message; if Unicode punctuation is allowed, document expected strength.
12. **Registration vs profile vs forgot password:** Same password strings applied in all three workflows → identical validation and strength behaviour (AC parity).
13. **Submit blocked:** Any invalid row from the table → create/change/reset **must not** persist until fixed (blocks creation per US3206).
14. **Regression:** After feature, existing login still works for migrated users with compliant passwords (ties to US3206 hashing ACs, separate from strength meter).

---

## Traceability

| US3206 acceptance theme | Covered by |
|-------------------------|------------|
| Validate against all policy rules | Rows 1–7, situations 1–6, 13. |
| Dynamic strength indicator | Situations 7–8, table valid rows. |
| Weak / Medium / Strong | Table strength column; situations 7–8. |
| Block creation if invalid | Situations 1–6, 13. |
| Violation message per rule | Situations 1–5. |
| All workflows | Situation 12. |
| Special characters supported | Table rows with `#`, `$`, `%`, `&`, `@`, `§`, `!`, `+`; situation 11 boundary. |
| Indicator placement | Out of scope for password strings; manual/UI check during execution. |

This outcome completes **Part 1** of the transfer task (policy analysis, multilingual password design, consolidated table) and extends it with explicit **test situations** for execution in the ToolShop.
