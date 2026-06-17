# 2.1.3 – Design a System Prompt for GitHub Copilot with Testing Guardrails

## Reference to the ISTQB Syllabus

**ISTQB GenAI – 2.1.3 System Prompts and User Prompts**

## Link to the Transfer Task

*https://github.com/rgroetz2/TBLL-AgileEngineeringFoundation/blob/main/courses/TestBusters-LearningLab/ISTQB-2026/genAI/transferTasks/Chapter2/2.1.3-System-Prompt-for-TTT426.md*

## Outcome

### Learning Summary

As part of this transfer task, a structured system prompt for GitHub Copilot was developed. The objective was to improve the quality, consistency, and safety of AI-generated test artifacts while avoiding common issues such as mixed test levels, missing test design foundations, or unsafe test data.

The developed system prompt defines GitHub Copilot as a Test Engineering Assistant and includes mandatory guardrails based on the TTT426 concepts of Test Type, Test Technique, Test Level, Test Data, Test Environment, and Test Automation.

Particular emphasis was placed on avoiding unsupported assumptions, handling uncertainties transparently, and providing standardized output formats. In addition, the prompt ensures that test design precedes test automation and that sensitive or production-related data is never used.

| Concept | Key Takeaway |
|----------|-------------|
| Role Definition | Copilot acts as a Test Engineering Assistant |
| Test Type | Every test must be assigned to a clearly defined test type |
| Test Level | Unit, Integration, System, and Acceptance Tests are clearly separated |
| Test Technique | Test cases are based on an explicitly stated test design technique |
| Test Data | Only realistic and non-sensitive test data may be used |
| Test Environment | Missing environment information must be explicitly identified |
| Test Automation | Test design must precede the generation of automation code |
| Uncertainty Handling | Missing information is made transparent and clarification is encouraged |
| Structured Outputs | Standardized formats promote consistency and traceability |

### Prompt Validation

The developed system prompt fulfills the requirements of the transfer task through the following characteristics:

- Improved consistency through mandatory output formats
- Reduced ambiguity through explicit test levels and test types
- Support for junior testers through clear structures and guidance
- Reduced hallucinations by identifying missing information
- Promotion of safe testing practices by excluding sensitive data
- Traceable test automation through the principle of “Test Design Before Automation”

### Conclusion

Creating a structured system prompt demonstrates how the behavior of an AI-assisted development tool can be guided and controlled. Clearly defined guardrails improve the quality of generated test artifacts while reducing the risk of incorrect, inconsistent, or unsafe outputs.

> **Key Takeaway:**
>
> A system prompt acts like a contract between the user and the AI.
>
> It defines roles, rules, quality expectations, and boundaries within which the AI should operate.
>
> The more precise these instructions are, the more consistent and reliable the generated results will be.