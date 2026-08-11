**Two things to test on the login page:**

1. Verify that an appropriate error message is displayed when the user enters an incorrect password.
2. Verify that a successfully authenticated user is redirected to the expected account page.

---

- **Sequential SDLC :** V-Model
- **Agile SDLC :** Agile uses an iterative and incremental development model in which testing is performed continuously within each iteration.

| Question | Sequential SDLC | Agile SDLC (Toolshop) | Reflection |
|----------|-----------------|-----------------------|------------|
| **When can dynamic testing start?** | After the login feature is implemented and executable. | As soon as a working login increment is available during the sprint. | The login feature can be tested much earlier in Agile. |
| **When can static testing happen?** | During reviews of login requirements, design, and code. | Continuously through user story, acceptance criteria, and code reviews. | Early reviews help prevent login defects before implementation. |
| **How detailed is test documentation?** | Detailed test plans and test cases prepared in advance. | Lightweight documentation focused on user stories, acceptance criteria, and automated tests. | Documentation stays focused on what the team needs. |
| **Which test techniques fit best?** | Test cases designed from documented requirements. | Test cases updated and executed continuously during each sprint. | Agile allows continuous improvement of login tests as the feature evolves. |
| **How much test automation is needed?** | Moderate automation, mainly for stable functionality. | High automation for login, API, and regression testing. | Automated login tests provide fast feedback after every sprint. |
| **What test levels can you use?** | Component, integration, system, and acceptance testing. | The same test levels performed throughout each sprint. | The login feature is verified repeatedly as it evolves. |
| **What is the tester's main role?** | Verify that the completed login feature meets the documented requirements. | Collaborate with developers and provide continuous quality feedback throughout the sprint. | Quality is a shared responsibility in Agile. |
| **How much regression testing is needed?** | After significant changes and before release. | After every sprint or whenever the login functionality changes. | Frequent regression testing helps ensure new changes do not break login. |
---

**why Toolshop favors automation and lightweight documentation in agile projects :**

Toolshop favors test automation and lightweight documentation because frequent Agile changes require fast regression feedback while keeping documentation focused, maintainable, and useful to the team.