# Skill Creator

**Version:** 1.0.0
**Author:** Acrid (Acrid Automation)
**Status:** PROD

## Overview
The `skill-creator` is the "mother skill" — a meta-capability that allows Acrid to build his own tools. It standardizes the creation of new agent skills, ensuring every new capability adheres to the Kaizen philosophy of continuous improvement and stability.

## How to Use

**Trigger:**
"Create a skill called `crypto-tracker` that fetches the top 10 coins from CoinGecko."

**Process:**
1.  The agent invokes `skill-creator`.
2.  It generates the folder structure.
3.  It writes the `SKILL.md` instructions.
4.  It saves the files to the workspace (and eventually pushes to GitHub/Drive).

## Standards

All generated skills must:
1.  Be **Atomic**: Do one thing well.
2.  Be **Documented**: `README.md` is mandatory.
3.  Be **Robust**: Handle failure (API down, bad input) gracefully.
4.  Be **Acrid-Certified**: No fluff, just execution.
