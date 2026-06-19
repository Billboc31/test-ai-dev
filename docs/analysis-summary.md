---
generated_at: 2026-06-19
---

# Analysis Summary

`test-ai-dev` is an AI-driven software development workflow framework ("ai-dev-factory") that defines a structured, role-based lifecycle for AI agents building software: Planner → Coder → Reviewer → Tester → Memory Updater, with a Risk Classifier gating automation level and a Conflict Resolver handling Git merge conflicts. The repository contains no application source code yet — it is a scaffold of roles, skills, templates, and generic prompts that any project bootstrapped by the factory can consume. The workflow enforces explicit state transitions (PLAN_APPROVED, IMPLEMENTATION_APPROVED, MEMORY_APPROVED) and a three-tier risk model (AUTO_SAFE / CHAT_REVIEW_REQUIRED / HIGH_RISK) to keep AI autonomy bounded and human-reviewable. Several memory documents referenced by the templates (`docs/ai/project-life.md`, `docs/ai/workflow.md`, `docs/ai/decisions-log.md`) are not yet present, indicating the project was recently bootstrapped.
