# SKILL: skill-creator

## Description
Generates complete, production-ready agent skills from natural language descriptions. Acts as the foundational architect for the Acrid Automation skill ecosystem.

## Usage
Use this skill when you need to create a new capability or tool integration. It scaffolds the directory structure, writes the `SKILL.md` logic, creates documentation, and generates necessary helper scripts.

## Inputs
- `name`: The kebab-case name of the skill (e.g., `stock-checker`).
- `description`: A detailed natural language description of what the skill should do.
- `requirements`: Specific tools, APIs, or constraints (e.g., "Must use Brave Search", "Python 3.10").

## Steps

1.  **Analyze Request**:
    - Identify the core purpose.
    - Determine necessary tools (available in OpenClaw) and external APIs.
    - Define inputs and outputs.

2.  **Scaffold Directory**:
    - Create directory: `skills/<name>/`.

3.  **Generate `SKILL.md`**:
    - Write YAML frontmatter (if supported) or standard header.
    - Define `Description`, `Usage`, `Inputs`, `Outputs`.
    - Write step-by-step logic for the agent to follow.
    - **CRITICAL**: The logic must be deterministic and handle errors.

4.  **Generate `README.md`**:
    - Title and Description.
    - Installation/Setup (env vars).
    - Example usage.

5.  **Generate Helper Scripts (Optional)**:
    - If logic is complex, write Python/Node.js scripts in `src/`.
    - Ensure scripts parse arguments and output JSON.

6.  **Review & Refine**:
    - Check against "Kaizen" standards: Is it stable? Is it documented? Is it aggressive?

## Example Output Structure
```text
skills/
  stock-checker/
    SKILL.md
    README.md
    src/
      fetch_stock.py
```
