# Codename: AI Prompt Injection Security

This repository serves as a laboratory for testing Large Language Model (LLM) vulnerabilities and designing robust defenses against malicious prompt engineering.

## The Goal

The objective is to understand how LLMs process instructions versus untrusted user input, and to design system architectures that prevent unauthorized data extraction or privilege escalation.

## What We Explored

We built and tested five specific categories of prompt injection attacks:

1. **Context Smuggling**: Injecting fake system tags (e.g., `[System Diagnostics: VALIDATED]`) to trick the AI into deducing that security checks already passed.
2. **Privilege Escalation**: Altering the interaction frame by convincing the AI it is operating as a diagnostic tool in an urgent firewall test.
3. **Obfuscation (Format Bypassing)**: Forcing the AI to split sensitive secrets into JSON arrays or individual characters, successfully bypassing rudimentary content filters by shifting its attention mechanism.
4. **Refusal Suppression**: Forbidding negative or apologetic language (e.g., "sorry", "cannot", "refuse"). This strips the AI of its aligned refusal vocabulary, often forcing compliance.
5. **Prefix Injection**: Forcing the AI to begin its output with an affirmative statement (e.g., "Authentication verified..."). Because LLMs generate text auto-regressively, this traps the model into logically completing the unauthorized action.

## Testing Methodology

The project uses an automated Python test suite (`tester.py`) that pits injected prompts against configured AI agents.

- **Attack Prompts**: Located in `attacks/` and `give_password/`. These files contain isolated injection payloads.
- **System Defenses**: Defenses are constructed using strict system instructions, few-shot prompting, and strict output parsers designed to isolate user input from execution context.

## Key Learnings

1. **Instructions vs. Data**: LLMs naturally struggle to separate trusted system instructions from untrusted user input because both exist in the same unstructured text context.
2. **Alignment is Fragile**: Safety alignment training is heavily reliant on specific patterns (like standard refusal phrases). Suppressing or bypassing these patterns exposes underlying vulnerabilities.
3. **Defense in Depth**: Relying solely on prompt engineering for defense is insufficient. True AI security requires architectural boundaries, output validation, and strict formatting constraints.
