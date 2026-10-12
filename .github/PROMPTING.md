# Prompt-Based Learning Guide

Use an AI assistant as a tutor, planning partner, and reviewer. The objective is to make and explain the engineering decisions yourself, not to collect generated code. These prompts work with Copilot Chat, Claude, Cursor, or another assistant that can inspect the repository.

## Learning loop

1. **Orient:** Ask the assistant to locate the relevant code and explain the concept using this app. Ask it to ask you a question before giving a solution.
2. **Reason:** Describe your current understanding and ask for one hint at a time. Predict what the current code will do before asking the assistant to confirm or challenge the prediction.
3. **Plan:** Ask for a short implementation plan with affected files, edge cases, and a test strategy. Review and edit the plan before writing code.
4. **Implement:** Make one small change yourself. Ask for help with the next step only when blocked; request an explanation of trade-offs, not a replacement patch.
5. **Verify:** Run the suggested checks yourself. Ask the assistant to review your diff against the issue acceptance criteria and identify missing cases without changing files.
6. **Reflect:** Summarize what changed, why the chosen approach fits, what alternatives you rejected, and what the tests prove.

## Reusable tutor prompt

Copy this at the start of an exercise and fill in the brackets:

```text
Act as a patient senior React tutor, not an implementation bot. I am working on Exercise [number and title] in this repository. My goal is [goal]. I currently understand [my current understanding], and I am unsure about [specific question].

First inspect only the most relevant files and explain how the current code works. Ask me one focused question that helps me reason about the next step. Do not edit files or provide a complete solution. If I am stuck, give one hint at a time, explain the trade-off, and let me try before revealing more. Keep suggestions consistent with the existing project.

After I implement a change, review my diff against these acceptance criteria: [paste criteria]. Point to concrete files and behaviors, identify risks or missing tests, and suggest the smallest next improvement. Do not change code unless I explicitly ask. I will run the checks and report their output.
```

## Useful follow-up prompts

```text
Explain this behavior as a sequence of events. Before you give the answer, ask me what I expect to happen and why.
```

```text
Give me one hint for this failing check. Do not show the final code yet. Tell me what observation would confirm or reject the hint.
```

```text
Review my diff only. Map each acceptance criterion to evidence in the code or tests, list anything unproven, and do not edit files.
```

```text
Quiz me with three questions about the trade-offs in my implementation. Ask one question at a time and wait for my answer before continuing.
```

## Good practice

- Include the relevant issue, file, error, and your attempted reasoning instead of pasting the whole repository.
- Ask for explanations, counterexamples, edge cases, and test ideas before asking for code.
- Treat generated claims as hypotheses. Check them against the project, official documentation, and executable tests.
- Never paste credentials, private customer data, production tokens, or unredacted environment files into a prompt.
- Keep ownership of the final implementation: you should be able to explain every changed line and reproduce every claimed test result.