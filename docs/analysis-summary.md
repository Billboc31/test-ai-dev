---
generated_at: 2026-06-19
---

# Analysis Summary

`test-ai-dev` is an AI-driven software development workflow framework ("ai-dev-factory") that defines a structured, role-based lifecycle for AI agents building software: Risk Classifier → Planner → Coder → Reviewer/Tester → Memory Updater, with a Conflict Resolver handling Git merge conflicts and a Deployer Fixer addressing sandbox failures. The repository contains no application source code — it is a scaffold of roles, skills, templates, and generic prompts that coordinate specialised AI agents in a bounded, auditable, and reversible process. The workflow enforces explicit state transitions (`PLAN_APPROVED`, `IMPLEMENTATION_APPROVED`, `MEMORY_APPROVED`) and a three-tier risk model (`AUTO_SAFE` / `CHAT_REVIEW_REQUIRED` / `HIGH_RISK`) to keep AI autonomy bounded and human-reviewable. The project was bootstrapped on 2026-06-18 and has not yet processed its first ticket; the core memory documents (`decisions-log.md`, `project-life.md`) have been initialised but `docs/ai/workflow.md` and the sandbox pipeline scripts remain to be created.
