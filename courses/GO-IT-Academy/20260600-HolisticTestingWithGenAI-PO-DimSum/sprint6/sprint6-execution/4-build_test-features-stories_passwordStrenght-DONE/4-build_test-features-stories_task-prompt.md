# **Title**

Hands-on: Design password-strength test situations (you write the prompts)


# **Background**

The **ToolShop** is the system under test. The sprint team is preparing validation of the **password strength** feature against the product’s password policy (minimum length, character classes including special characters, and strength levels as defined in product materials). Test situations are **isolated conditions** under which the test object should show specific behaviour. This handout is intentionally **not** a 1:1 prompt: learners must decide goals, context, output shape, and follow-up questions themselves.

# **Reference**

- Course transfer task: `sprint6-execution/4-build_test-features-stories_passwordStrenght-DONE/4-build_test-features-stories_task.md`
- User story (password policy / hashing context per course materials): `sprint6/sprint6-input/US3206-hashed-password.md` 

# **Tasks**

1. Read the user story and any ToolShop UI/API notes for the password feature; derive exact policy wording, strength labels, and edge cases.

2. Author **prompt A** for one model/provider, asking for test situations in a way you judge will yield testable passwords and clear expected results.

3. OPTIONAL: Author **prompt B** for a **different** model or provider (or a clearly different instruction style); do not reuse one mega-prompt verbatim everywhere—adapt to learn what changes the answer.

4. Ensure coverage includes: **policy violations one rule at a time**; **valid** passwords with **different strength** where the product defines levels; **invalid** examples; passwords reflecting **German, Czech, and English** character content, using **special characters typical of those languages** where relevant.

5. Consolidate into **your** table (or equivalent): password, expected strength, valid/invalid—only as far as the product definition allows.

6. Submit **your two prompts** (as you used them), the consolidated artefact, and a **short reflection** on differences between models (coverage, edge cases, language/special characters, format, errors) and what you would change next time.

7. Exercise a subset against the running ToolShop and record mismatches between expected and actual behaviour.
